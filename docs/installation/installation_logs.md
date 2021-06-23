---
layout: default
title: Configuring Logs
parent: Installation
nav_order: 2
permalink: /docs/installation/logs
baselevel: 2
---

# Configuration KARNAK logs

KARNAK offers the possibility of injecting your own log configuration.

The logging system used by KARNAK is [logback](http://logback.qos.ch/). For more details on logback, it provides a [manual](http://logback.qos.ch/manual/index.html)

## Variables

Some variables are available and can be used in the logback file.

* `issuerOfPatientID` are the issuer of patient ID **before** the deidentification
* `PatientID` are the patient ID **before** the deidentification
* `SOPInstanceUID` are the SOPInstanceUID **before** the deidentification
* `DeidentifySOPInstanceUID` are the SOPInstanceUID **after** the deidentification
* `SeriesInstanceUID` are the SeriesInstanceUID **before** the deidentification
* `DeidentifySeriesInstanceUID` are the SeriesInstanceUID **after** the deidentification
* `ProjectName` are the project name used for the deidentification
* `ProfileName` are the profile name used for the deidentification
* `ProfileCodenames` are a list of concatenated profile items that has been used for the deidentification

## Inject the logback configuration file

### By using docker

You must set the environment variable `LOGBACK_CONFIGURATION_FILE` with the path of Logback file configuration, it will override the default log file.

For example, if you create your own configuration with the file name `my-logback.xml` at the root of the docker-compose.yml.

You must create a volume that will copy the file in the KARNAK docker and define the environment variable `LOGBACK_CONFIGURATION_FILE` with the path of the file in the docker container.

```
services:
  karnak:
    container_name: karnak
    image: osirixfoundation/karnak:v0.9.7
    volumes:
      - ./my-logback.yml:/logs/my-logback.xml
    environment:
      LOGBACK_CONFIGURATION_FILE: /logs/my-logback.xml
```

### By using JAVA

If you use directly the KARNAK jar, you must add the following parameter at the startup:

`-Dlogging.config=my-logback.xml`

## Default logback file

The default [logback file](https://github.com/OsiriX-Foundation/karnak/blob/master/src/main/resources/logback.xml) (see below) has two modes.

### Dev Mode

* The variable `ENVIRONMENT` must be set to `DEV` to be activate.
* Log everything at the `INFO` level except for the package `org.karnak` and `org.weasis`, they are at the `DEBUG` level.

### Production Mode

* Always activate, except if the variable `ENVIRONMENT` is set to `DEV`.
* Logs everything at the `WARN` level
* Writes all.log file, it contains:
  * Every `WARN` level log
  * `org.weasis` `INFO` level log
  * `org.karnak` `INFO` level log, excepted clinical log
* Writes clinical.log, it contains:
  * Every log with the marker `CLINICAL` which concerns the deidentification

```xml

<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <property name="LOGS" value="logs" />
    <!--
    DEV LOGS
    Condition to verify if "ENVIRONMENT" is "DEV"
    -->
    <if condition='property("ENVIRONMENT").contains("DEV")'>
        <then>
            <!--
            How the logs will be displayed in the standard output (console)
            SOPInstanceUID, issuerOfPatientID and PatientID are variables to associate the current DICOM with the log
            -->
            <appender name="DEV_OUT" class="ch.qos.logback.core.ConsoleAppender">
                <encoder>
                    <Pattern>%black(%d{ISO8601})  %highlight(%-5level) %marker %highlight(%X{SOPInstanceUID}) %highlight(%X{issuerOfPatientID}) %highlight(%X{PatientID}) [%yellow(%t)] %yellow(%C{1.}): %msg%n%throwable</Pattern>
                </encoder>
            </appender>
            <!--
            Log everything at the INFO level except for the package org.karnak and org.weasis, they are at the DEBUG level.
            -->
            <root level="info">
                <appender-ref ref="DEV_OUT" />
            </root>
            <logger name="org.weasis" level="debug" />
            <logger name="org.karnak" level="debug" />
        </then>
        <!--
        PRODUCTION LOGS
        -->
        <else>
            <!--
            How the warning logs will be displayed in the standard output (console)
            SOPInstanceUID, issuerOfPatientID and PatientID are variables to associate the current DICOM with the log
            -->
            <appender name="WARNING_OUT" class="ch.qos.logback.core.ConsoleAppender">
                <!--
                Log only at WARN level
                -->
                <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
                    <level>WARN</level>
                </filter>
                <encoder>
                    <Pattern>%black(%d{ISO8601})  %highlight(%-5level) %marker %highlight(%X{SOPInstanceUID}) %highlight(%X{issuerOfPatientID}) %highlight(%X{PatientID}) [%yellow(%t)] %yellow(%C{1.}): %msg%n%throwable </Pattern>
                </encoder>
            </appender>
            <!--
            Write all.log file in the file system.
            -->
            <appender name="ALL_LOGS" class="ch.qos.logback.core.rolling.RollingFileAppender">
                <file>${LOGS}/all/all.log</file>
                <!--
                Window rolling policy defined by the variable KARNAK_LOGS_MIN_INDEX or KARNAK_LOGS_MAX_INDEX
                By default the min is 1 and the max is 10
                -->
                <rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
                    <fileNamePattern>${LOGS}/all/all_%i.log</fileNamePattern>
                    <minIndex>${KARNAK_LOGS_MIN_INDEX:-1}</minIndex>
                    <maxIndex>${KARNAK_LOGS_MAX_INDEX:-10}</maxIndex>
                </rollingPolicy>
                <!--
                Size rolling policy defined by the variable KARNAK_LOGS_MAX_FILE_SIZE
                By default the max file size is 50MB
                -->
                <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
                    <maxFileSize>${KARNAK_LOGS_MAX_FILE_SIZE:-50MB}</maxFileSize>
                </triggeringPolicy>
                <!--
                Filter not to write logs with the clinical marker
                -->
                <filter class="ch.qos.logback.core.filter.EvaluatorFilter">
                    <evaluator class="ch.qos.logback.classic.boolex.OnMarkerEvaluator">
                        <marker>CLINICAL</marker>
                    </evaluator>
                    <onMismatch>NEUTRAL</onMismatch>
                    <onMatch>DENY</onMatch>
                </filter>
                <encoder>
                    <pattern>%d %-5level %m%n</pattern>
                </encoder>
            </appender>
            <!--
            Write clinical.log file in the file system.
            Will contains only the logs with the CLINICAL marker. The logs with CLINICAL marker concerns the information about the deidentification.
            Variables used for the CLINICAL log:
            SOPInstanceUID, DeidentifySOPInstanceUID, SeriesInstanceUID, DeidentifySeriesInstanceUID, ProjectName, ProfileName, ProfileCodenames
			-->
            <appender name="CLINICAL_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
                <file>${LOGS}/Clinical/clinical.log</file>
                <rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
                    <fileNamePattern>${LOGS}/Clinical/clinical_%i.log</fileNamePattern>
                    <minIndex>${KARNAK_CLINICAL_LOGS_MIN_INDEX:-1}</minIndex>
                    <maxIndex>${KARNAK_CLINICAL_LOGS_MAX_INDEX:-10}</maxIndex>
                </rollingPolicy>

                <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
                    <maxFileSize>${KARNAK_CLINICAL_LOGS_MAX_FILE_SIZE:-50MB}</maxFileSize>
                </triggeringPolicy>

                <filter class="ch.qos.logback.core.filter.EvaluatorFilter">
                    <evaluator class="ch.qos.logback.classic.boolex.OnMarkerEvaluator">
                        <marker>CLINICAL</marker>
                    </evaluator>
                    <onMismatch>DENY</onMismatch>
                    <onMatch>NEUTRAL</onMatch>
                </filter>
                <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
                    <Pattern>%d SOPInstanceUID_OLD=%X{SOPInstanceUID} SOPInstanceUID_NEW=%X{DeidentifySOPInstanceUID} SeriesInstanceUID_OLD=%X{SeriesInstanceUID} SeriesInstanceUID_NEW=%X{DeidentifySeriesInstanceUID} ProjectName=%X{ProjectName} ProfileName=%X{ProfileName} ProfileCodenames=%X{ProfileCodenames}</Pattern>
                </encoder>
            </appender>

            <!--
            Log everything at the WARN level except for the package org.karnak and org.weasis, they are at the INFO level.
            The INFO logs won't appear in the standard output, because they will be filtered by the WARNING_OUT appender
            -->
            <root level="warn">
                <appender-ref ref="ALL_LOGS" />
                <appender-ref ref="CLINICAL_FILE" />
                <appender-ref ref="WARNING_OUT" />
            </root>
            <logger name="org.weasis" level="info" />
            <logger name="org.karnak" level="info" />
        </else>
    </if>
</configuration>
```

