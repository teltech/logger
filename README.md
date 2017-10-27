# teltech/logger

Super simple structured logging mechanism for Go projects with [Stackdriver format](https://cloud.google.com/error-reporting/docs/formatting-error-messages) compatibility

## Installation

``` sh
go get -u github.com/teltech/logger
```

## Usage
``` go
package main

import (
    "github.com/teltech/logger"
)

// There should be a LOG_LEVEL environment variable set, which is read by the library
// If no value is set, the default LOG_LEVEL will be INFO

func main() {
    // Stackdriver requires a project name and   version to be set. Use your environment for these values.
    // SERVICE should be your GCP project-id, e.g. robokiller-146813
    // VERSION is an arbitrary value
    log := log.New()

    // You can also initialize the logger with a context, the values will persisted throughout the scope of the logger instance
    log := log.New().WithContext(Fields{
        "user": "+1234567890",
        "action": "create-account",
    })

    // A metric is an INFO log entry without a payload
    log.Metric("CUSTOM_METRIC_ENTRY")

    // Log a DEBUG message, only visible in when LOG_LEVEL is set to DEBUG
    log.With(log.Fields{"key":"val"}).Debug("debug message goes here")

    // Log an INFO message
    log.With(log.Fields{"key":"val"}).Info("info message goes here")

    // Log a WARN message
    log.With(log.Fields{"key":"val"}).Warn("warn message goes here")

    // Error() prints the stacktrace as part of the payload for each entry and sends the
    // data to Stackdriver Error Reporting service
    log.With(log.Fields{"key":"val"}).Error("error message goes here")
}