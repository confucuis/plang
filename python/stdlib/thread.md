### threading.Thread

- **多线程**
```python
import time
import queue
import threading


class TaskProducer(threading.Thread):
    def __init__(self, name: str, q: queue.Queue):
        threading.Thread.__init__(self)
        self.q = q
        self.name = name

    def run(self):
        for i in range(10):
            task = {"name": self.name, "id": i}
            print(f"producer: {task}")
            self.q.put(task)


class TaskConsumer(threading.Thread):
    def __init__(self, name: str, q: queue.Queue):
        threading.Thread.__init__(self)
        self.q = q
        self.name = name

    def run(self):
        while True:
            task = self.q.get()
            print(f"consumer: {task}")
            if self.q.empty():
                break
            time.sleep(1.5)


if __name__ == "__main__":
    q = queue.Queue(maxsize=5)
    producer = TaskProducer("生产者", q)
    consumer = TaskConsumer("消费者", q)

    producer.start()
    consumer.start()

    producer.join()
    consumer.join()

    print("===========================")

```