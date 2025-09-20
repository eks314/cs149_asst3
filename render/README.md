
# Solution

Given

```
Device settings: SM=40, Threads=1024, WarpsPerSM=32,
sharedMemPerBlock=48(kb), sharedMemPerMultiprocessor=64(kb),
deviceTotalGlobalMem=14(gb), FP32Flops=4070400
```

Let's define one unit of work.

One unit of work is colouring a square of 32 x 32 pixels with not more than 100 circles.

Top-level task is to prepare such unit of work input.

When it is prepared, it should be executed.

Execution has one limitation. You cannot concurrently process the same rectangle.

So we create a queue of tasks.

In this queue there is some task that repeats the same rectangle. Before doing it we should complete all previous tasks for simplicity.

```
class Queue:
	self.tasks = []
	self.deferred_tasks = []
	self.max_deferred_tasks = 100


while not queue.empty():
	active_rectangles = set()
	next_tasks = []
	next_tasks_limit = SM_count
	while not queue.empty() and len(next_tasks) < next_tasks_limit:
		task = queue.peek()
		if task.rectangle not in active_rectangles:
			next_tasks.append(queue.pop())
		else:
			success = queue.defer_next()
			if not success:
				break;
	execute(next_tasks)
	synchronize()
```

