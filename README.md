# Library for structured logging in Python

## Examples

Simple script for starting:

```python
from simple_json_logging import init_json_logger

logger = init_json_logger('my_json_logger')


def main():
    logger.warning('Test log message with arg1 %s and arg2 %s', 'abc', 100, foo='yyy', bar=100500)
    try:
        raise RuntimeError('just for test')
    except Exception:
        logger.exception('exception', foo='yyy')


if __name__ == '__main__':
    main()
```

This script will output:

```
{"name": "my_json_logger", "levelname": "WARNING", "levelno": 30, "pathname": "example.py", "filename": "example.py", "module": "example", "stack_info": null, "lineno": 7, "funcName": "main", "created": 1564591269.778053, "msecs": 778.0530452728271, "relativeCreated": 4.003047943115234, "thread": 30272, "threadName": "MainThread", "processName": "MainProcess", "process": 19180, "data": {"foo": "yyy", "bar": 100500}, "message": "Test log message with arg1 abc and arg2 100"}
{"name": "my_json_logger", "levelname": "ERROR", "levelno": 40, "pathname": "example.py", "filename": "example.py", "module": "example", "stack_info": null, "lineno": 11, "funcName": "main", "created": 1564591269.778053, "msecs": 778.0530452728271, "relativeCreated": 4.003047943115234, "thread": 30272, "threadName": "MainThread", "processName": "MainProcess", "process": 19180, "data": {"foo": "yyy"}, "message": "exception", "exceptionClass": "RuntimeError", "exceptionMessage": "just for test"}
```

Another one with a custom formatter:

```python
from simple_json_logging import JsonFormatter, init_json_logger

formatter = JsonFormatter(json_dumps_args={'sort_keys': True, 'indent': 2})
logger = init_json_logger('my_json_logger', formatter=formatter)


def main():
    logger.warning('Test log message with arg1 %s and arg2 %s', 'abc', 100, foo='yyy', bar=100500)
    try:
        raise RuntimeError('just for test')
    except Exception:
        logger.exception('exception', foo='yyy')


if __name__ == '__main__':
    main()
```

This script will output:
```
{
  "created": 1564591638.1781173,
  "data": {
    "bar": 100500,
    "foo": "yyy"
  },
  "exc_text": null,
  "filename": "example.py",
  "funcName": "main",
  "levelname": "WARNING",
  "levelno": 30,
  "lineno": 8,
  "message": "Test log message with arg1 abc and arg2 100",
  "messageFormatted": "Test log message with arg1 abc and arg2 100",
  "module": "example",
  "msecs": 178.1172752380371,
  "name": "my_json_logger",
  "pathname": "example.py",
  "process": 26544,
  "processName": "MainProcess",
  "relativeCreated": 2.9993057250976562,
  "stack_info": null,
  "thread": 28208,
  "threadName": "MainThread"
}
{
  "created": 1564591638.1781173,
  "data": {
    "foo": "yyy"
  },
  "exc_text": "Traceback (most recent call last):\n  File \"example.py\", line 10, in main\n    raise RuntimeError('just for test')\nRuntimeError: just for test",
  "exceptionClass": "RuntimeError",
  "exceptionMessage": "just for test",
  "filename": "example.py",
  "funcName": "main",
  "levelname": "ERROR",
  "levelno": 40,
  "lineno": 12,
  "message": "exception",
  "messageFormatted": "exception",
  "module": "example",
  "msecs": 178.1172752380371,
  "name": "my_json_logger",
  "pathname": "example.py",
  "process": 26544,
  "processName": "MainProcess",
  "relativeCreated": 2.9993057250976562,
  "stack_info": null,
  "thread": 28208,
  "threadName": "MainThread"
}
```

## Known issues

Didn't tested on versions before Python 3.7, please give me feedback about previous versions.
