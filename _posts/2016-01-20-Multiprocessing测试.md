---
layout:     post
title:      "Multiprocessing测试"
subtitle:   ""
date:       2016-01-21 17:00:00
author:     "xiaobai"
header-img: "img/post-bg-06.jpg"
---

###Python 并发请求性能比较

网上很多都是介绍生产者-消费者模型来做并发
所以根据网上例子做一下测试

```python
def Producer():
    urls = [
        'http://www.python.org',
        'https://segmentfault.com/a/1190000000382873',
        'http://ddt.8888play.com/news_3935.html',
        'http://douban.fm/',
        'http://docs.jinkan.org/docs/celery/getting-started/introduction.html',
        'http://formspree.io/account',
        'http://46.101.85.29/index/#/mz/multipletextsearch/',
        'https://pandao.github.io/editor.md/',
        'https://www.zhihu.com/question/21343711',
        'https://docs.python.org/3/library/queue.html',
        'http://www.python.org',
        'http://www.python.org',
    ]
    queues = queue.Queue()
    pool_size=int(multiprocessing.cpu_count()*0.5)
    print('线程池线程数量：'+str(pool_size))
    worker_threads = build_worker_pool(queues, pool_size)
    start_time = time.time()
    results = []
    for url in urls:
        queues.put(url)
    for worker in worker_threads:
        queues.put('quit')
    for worker in worker_threads:
        worker.join()

    print('Done! Time taken:{}'.format(time.time() - start_time))


def build_worker_pool(queues, size):
    workers = []
    for _ in range(size):
        worker = Consumer(queues)
        worker.start()
        workers.append(worker)
    return workers

if __name__ == '__main__':
    Producer()
```

#####线程池pool_size 分别使用 8,16,32进行测试

#####测试结果如下图所示
![](https://raw.githubusercontent.com/xiaobaiyizhi/xiaobaiyizhi.github.io/master/img/process-test/c-p8%20.png)

![](https://raw.githubusercontent.com/xiaobaiyizhi/xiaobaiyizhi.github.io/master/img/process-test/c-p16.png)

![](https://raw.githubusercontent.com/xiaobaiyizhi/xiaobaiyizhi.github.io/master/img/process-test/c-p32.png)