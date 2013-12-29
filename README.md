Logback Custom Highlighting Color
====================

logback provides `%highlight` keyword for coloring output based on log level. It's a very useful but less known feature. One problem is that the default color scheme is too dark to read on a black/dark console.

Here is a converter allowing you to customize the color scheme. What you need to do is simply:

1. Add `HighlightingCompositeConverterEx.java` to CLASSPATH (or just add it to your project)
1. load the converter class in `logback.xml` by using `<conversionRule>`
1. Use your conversion word (`highlightex` in the example) in `<pattern>`

```Java
package com.acme.logback;

public class HighlightingCompositeConverterEx extends ForegroundCompositeConverterBase<ILoggingEvent> {

    @Override
    protected String getForegroundColorCode(ILoggingEvent event) {
        Level level = event.getLevel();
        switch (level.toInt()) {
        case Level.ERROR_INT:
            return ANSIConstants.BOLD + ANSIConstants.RED_FG; // same as default color scheme
        case Level.WARN_INT:
            return ANSIConstants.RED_FG;// same as default color scheme
        case Level.INFO_INT:
            return ANSIConstants.CYAN_FG; // use CYAN instead of BLUE
        default:
            return ANSIConstants.DEFAULT_FG;
        }
    }

}
```

```XML
<!-- logback.xml -->
<configuration>
    <!-- custom coloring conversion -->
    <conversionRule conversionWord="highlightex" converterClass="com.acme.logback.HighlightingCompositeConverterEx" />

    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <withJansi>true</withJansi>
        <encoder>
            <pattern>[%thread] %highlightex(%-5level) %logger{15} - %highlightex(%msg) %n</pattern>
        </encoder>
    </appender>

    <root level="DEBUG">
        <appender-ref ref="STDOUT" />
    </root>
</configuration>
```
