# Cloud logger

An experimental lightweight logger. The package was supposed to be called **cloud_logger** but because of a name conflict on PyPi it is called **cloud_logger2** there. After the package is installed all imports still must be done from **cloud_logger** but this can be changed in the future.

This logger is a very simple and lightweigth and supports only output to **stdout** and **stderr**. No extra features or things like coloring are supported. The main purpose of this logger is to shoot log messages as fast as possible inside containerized applications in clouds.

The logger has a minimalistic configuration (say no configuration at all) or can be controlled by several environment varables. The supported envirnment variables are:
- CLOUD_LOG_LEVEL - set the logger level. The level can be represented as one of "debug", "info", "warning" or "error" values.
- CLOUD_LOG_JSON - enable output as JSON.
- CLOUD_LOG_FORMAT - define string output format, for example, "{level} {created_at} {message}".
- CLOUD_LOG_JSON_FIELDS - define a list of fields in a JSON output, for example, "level,created_at,message".

The output can be formatted in several styles:
- structured key=value pairs
- arbitrary log string
- structured JSON strings

## Usage exampes
```python
import os

# Use default logger
import cloud_logger as logger

logger.bind('process_id', os.getpid())

logger.debug('Debug message')
logger.info('Info message')
logger.warning('Warning message', status=404)
logger.error('Error message', error=500)

>>> level="info" message="Info message" created_at="2020-07-27T17:43:15.571740" process_id="7674"
>>> level="warning" message="Warning message" created_at="2020-07-27T17:43:15.571795" process_id="7674" status="404"
>>> level="error" message="Error message" error="500" created_at="2020-07-27T17:43:15.571820" process_id="7674"
```
The debug message is not shown because the default level is INFO. The default output is structured as key=value pairs.

```python
from cloud_logger import CloudLogger

log = CloudLogger(log_format='{created_at} {message}')

for idx in range(3):
    log.info(idx)

>>> 2020-07-27T17:55:57.803640 0
>>> 2020-07-27T17:55:57.803678 1
>>> 2020-07-27T17:55:57.803691 2
```
