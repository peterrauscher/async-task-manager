# async-task-manager
async-task-manager is a Python library that provides an easy-to-use interface for managing asyncio tasks with dependencies. It allows you to create tasks, specify their dependencies, and run them concurrently in a thread pool.

## Features

- Create tasks with unique task IDs and coroutine functions
- Specify dependencies between tasks
- Run tasks concurrently, respecting the maximum thread count
- Ensure tasks are run only after all their dependencies have finished running

## Installation

You can install Asyncio Task Manager using pip:

```
pip install asyncio-task-manager
```

## Methods
TaskManager(max_threads: int)
Creates a new task manager instance with the specified maximum thread count.

create_task(task_id: str, coroutine: Callable[..., Coroutine], *args, **kwargs) -> None
Creates a new task with the given task ID and coroutine function.

add_dependency(task_id: str, dependency_id: str) -> None
Adds a dependency between two tasks, indicating that task_id depends on the completion of dependency_id.

run_tasks() -> None
Runs all tasks in the dependency graph, respecting the maximum thread count.

## Example Usage

Here's a basic example of how to use Asyncio Task Manager:

```python
from asyncio_task_manager import TaskManager
import asyncio

async def task1():
    print("Running task 1")
    await asyncio.sleep(1)
    print("Task 1 completed")

async def task2():
    print("Running task 2")
    await asyncio.sleep(2)
    print("Task 2 completed")

async def task3():
    print("Running task 3")
    await asyncio.sleep(1)
    print("Task 3 completed")

async def main():
    task_manager = TaskManager(max_threads=2)

    task_manager.create_task("task1", task1)
    task_manager.create_task("task2", task2)
    task_manager.create_task("task3", task3)

    task_manager.add_dependency("task2", "task1")
    task_manager.add_dependency("task3", "task1")

    await task_manager.run_tasks()

asyncio.run(main())
```

In this example, we create three tasks (task1, task2, and task3) and specify that task2 and task3 depend on task1. We set the maximum thread count to 2. The task manager ensures that task1 is run first, and then task2 and task3 are run concurrently.

## Contributing
Contributions are welcome! If you find any issues or have suggestions for improvements, please open an issue or submit a pull request on the GitHub repository.

## License
This library is licensed under the MIT License.
