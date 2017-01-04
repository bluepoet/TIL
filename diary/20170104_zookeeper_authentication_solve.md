# 주키퍼 인증관련 문제 해결 스토리

## jvm 옵션으로 해결시도

기존에 kafka client 라이브러리를 0.8.2.2 버전을 쓰고 있었는데 0.10.1.1으로 올리고, consumer모듈 서버를 올리니 인증관련 에러가 나서 JaasUtils.java 소스를 보고 jvm옵션을 넣어 로컬에서 잘 돌아가는 거 확인
-Dzookeeper.sasl.client=false -Dzookeeper.sasl.clientconfig=""

```
public static boolean isZkSecurityEnabled() {
        boolean zkSaslEnabled = Boolean.parseBoolean(System.getProperty(ZK_SASL_CLIENT, "true"));
        String zkLoginContextName = System.getProperty(ZK_LOGIN_CONTEXT_NAME_KEY, "Client");

        boolean isSecurityEnabled;
        try {
            Configuration loginConf = Configuration.getConfiguration();
            isSecurityEnabled = loginConf.getAppConfigurationEntry(zkLoginContextName) != null;
        } catch (Exception e) {
            throw new KafkaException("Exception while loading Zookeeper JAAS login context '" + zkLoginContextName + "'", e);
        }
        if (isSecurityEnabled && !zkSaslEnabled) {
            LOG.error("JAAS configuration is present, but system property " +
                        ZK_SASL_CLIENT + " is set to false, which disables " +
                        "SASL in the ZooKeeper client");
            throw new KafkaException("Exception while determining if ZooKeeper is secure");
        }

        return isSecurityEnabled;
    }
```

하지만 운영서버에 올리니 여전히 인증설정 관련 에러 발생해 소스를 보던 중, 전에 발송로그도 secure/unsecure zookeeper cluster 접속시 인증관련 문제가 생긴 걸 기억하고
secure -> unsecure zookeeper로 접속하던 로직을 unsecure -> secure로 바꾸니 서버는 잘 올라감. 앞뒤 전후관계를 엮는 논리력이 필요하고 그 논리력의 기반인 기본기가 충실해야 됨을 느꼈다.

## 어플리케이션 배포 후 서버를 올리니 아래와 같은 warning 로그 발견

하지만 서버 시작 후 아래와 같은 모듈별 warning 로그 발견

producer 모듈(기존 0.8.2.2 라이브러리에서 0.10.1.0 클러스터 접속시 발생하고 있었음)
참고 : https://github.com/apache/kafka/pull/1166
```
2016-12-14 11:01:49,927 WARN o.a.kafka.common.network.Selector - Error in I/O with alpha-kafka005.dakao.io/10.60.28.89
java.io.EOFException: null
at org.apache.kafka.common.network.NetworkReceive.readFrom(NetworkReceive.java:62)
at org.apache.kafka.common.network.Selector.poll(Selector.java:248)
at org.apache.kafka.clients.NetworkClient.poll(NetworkClient.java:192)
at org.apache.kafka.clients.producer.internals.Sender.run(Sender.java:191)
at org.apache.kafka.clients.producer.internals.Sender.run(Sender.java:122)
at java.lang.Thread.run(Thread.java:745)
```

consumer 모듈(0.10.1.0 버전업 후 발생)
```
SASL configuration failed: javax.security.auth.login.LoginException: Zookeeper client cannot authenticate using the 'Client' section of the supplied JAAS configuration: '/xxx-client.jaas' because of a RuntimeException: java.lang.SecurityException: java.io.IOException: Configuration Error:
	Line 6: expected [controlFlag] Will continue connection to Zookeeper server without SASL authentication, if Zookeeper server allows it.
```

producer모듈쪽 warning로그는 라이브러리 버전업으로 해결했고, consumer쪽은 회사동료의 도움으로 주키퍼 소스를 보고 시스템 프로퍼티를 추가해 해결했다.
오픈소스를 많이 쓰는 만큼, 소스를 읽는 능력도 중요함을 경험했다.
System.setProperty("zookeeper.sasl.client", "false");

https://github.com/apache/zookeeper/blob/601207e1151b2691112c431fc3b4130a85ac93b5/src/java/main/org/apache/zookeeper/client/ZooKeeperSaslClient.java#L76-L78

https://github.com/apache/zookeeper/blob/601207e1151b2691112c431fc3b4130a85ac93b5/src/java/main/org/apache/zookeeper/ClientCnxn.java#L945-L964

```
if (ZooKeeperSaslClient.isEnabled()) {
                try {
                    String principalUserName = System.getProperty(
                            ZK_SASL_CLIENT_USERNAME, "zookeeper");
                    zooKeeperSaslClient =
                        new ZooKeeperSaslClient(
                                principalUserName+"/"+addr.getHostName());
                } catch (LoginException e) {
                    // An authentication error occurred when the SASL client tried to initialize:
                    // for Kerberos this means that the client failed to authenticate with the KDC.
                    // This is different from an authentication error that occurs during communication
                    // with the Zookeeper server, which is handled below.
                    LOG.warn("SASL configuration failed: " + e + " Will continue connection to Zookeeper server without "
                      + "SASL authentication, if Zookeeper server allows it.");
                    eventThread.queueEvent(new WatchedEvent(
                      Watcher.Event.EventType.None,
                      Watcher.Event.KeeperState.AuthFailed, null));
                    saslLoginFailed = true;
                }
            }
```