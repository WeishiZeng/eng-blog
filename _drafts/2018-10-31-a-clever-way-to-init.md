https://www.geeksforgeeks.org/anonymous-inner-class-java/

https://stackoverflow.com/questions/9625297/initializing-hashtables-in-java



public class TrackerProcessorImplFactoryTest {

 private static Properties TRACKING_PROCESSOR_CONFIG = new Properties() {
   {
     setProperty("group.id", "tscpExperimentTrackingConsumer");
     setProperty("zookeeper.connect", "zk-lit-lca1-1-kafka.corp.linkedin.com:12913/kafka-lit");
     setProperty("zookeeper.connection.timeout.ms", "10");
     setProperty("zookeeper.session.timeout.ms", "60");
     setProperty("auto.commit.enable", "true");
     setProperty("auto.commit.interval.ms", "10");
     setProperty("rebalance.max.retries", "5");
     setProperty("rebalance.backoff.ms", "180000");
     setProperty("socket.timeout.ms", "3600");
     setProperty("offsets.storage", "zookeeper");
     setProperty("dual.commit.enabled", "false");
   }};
