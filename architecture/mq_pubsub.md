# MessageQueueモデルとPub/Subモデルについて

Pub/Subモデル(以下pubsub)はMessageQueueモデル(以下MQ)のスーパーセット的な位置付け。

MQが単なるメッセージをenqueue/dequeueするものと見た場合、pubsubはdequeue側にsubscriptionという層を1枚挟んだ形となる。
例えばgoogleのcloud pub/subでは以下のような実装となっている。

![pub/sub image](https://cloud.google.com/pubsub/images/many-to-many.svg)

ここでtopicはMQで言うところのqueue種別を表すようなもの。
Publisherは指定のTopic上にメッセージを送信していく。メッセージはSubscriptionに対して転送され、SubscriverはSubscriptionからメッセージを受信する。

Subscriptionは複数のSubscriberから接続出来る。これによりMQでは通常コンシューマ1台あたりがメッセージを受信した時点で削除されるものが、複数のSubscriberに対して配布可能となる。

![pub/sub flow](https://cloud.google.com/pubsub/images/pub_sub_flow.svg)

#### 参考リンク

* [メッセージキューモデル](https://ja.wikipedia.org/wiki/%E3%83%A1%E3%83%83%E3%82%BB%E3%83%BC%E3%82%B8%E3%82%AD%E3%83%A5%E3%83%BC)
* [Pub/Subモデル](https://ja.wikipedia.org/wiki/%E5%87%BA%E7%89%88-%E8%B3%BC%E8%AA%AD%E5%9E%8B%E3%83%A2%E3%83%87%E3%83%AB)
* [google cloud pub/sub](https://cloud.google.com/pubsub/docs/)
