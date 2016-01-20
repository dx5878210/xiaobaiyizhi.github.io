---
layout:     post
title:      "Multiprocessing����"
subtitle:   ""
date:       2016-01-21 17:00:00
author:     "xiaobai"
header-img: "img/post-bg-06.jpg"
---

###Python �����������ܱȽ�

���Ϻܶ඼�ǽ���������-������ģ����������
���Ը�������������һ�²���

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
    print('�̳߳��߳�������'+str(pool_size))
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

#####�̳߳�pool_size �ֱ�ʹ�� 8,16,32���в���

#####���Խ������ͼ��ʾ
![](https://raw.githubusercontent.com/xiaobaiyizhi/xiaobaiyizhi.github.io/master/img/process-test/c-p8%20.png)

![](https://raw.githubusercontent.com/xiaobaiyizhi/xiaobaiyizhi.github.io/master/img/process-test/c-p16.png)

![](https://raw.githubusercontent.com/xiaobaiyizhi/xiaobaiyizhi.github.io/master/img/process-test/c-p32.png)