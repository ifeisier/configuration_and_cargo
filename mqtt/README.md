# MQTT

快速创建 rumqttc 客户端.


## 使用

```rust
use std::time::Duration;
use mqtt::Client;
use mqtt::client::DefaultClient;
use tokio::time;

fn main() {
  let runtime = new_current_thread().unwrap();
  runtime.block_on(async {
    let (client, mut event_loop) = DefaultClient::new("id", "192.168.200.217", 1883);
    client.subscribe("hello/rumqtt", 2).unwrap();

    tokio::spawn(async move {
      for i in 0..10 {
        client
        .publish("hello/rumqtt", 2, vec![i; i as usize])
        .unwrap();
        time::sleep(Duration::from_millis(100)).await;
      }
    });

    loop {
      let notification = event_loop.poll().await.unwrap();
      println!("Received = {:?}", notification);
    }
  });
}
```


