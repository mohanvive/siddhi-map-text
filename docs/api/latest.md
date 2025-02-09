# API Docs - v2.0.2

## Sinkmapper

### text *<a target="_blank" href="https://siddhi.io/en/v5.0/docs/query-guide/#sink-mapper">(Sink Mapper)</a>*

<p style="word-wrap: break-word">This extension is a Event to Text output mapper. Transports that publish text messages can utilize this extension to convert the Siddhi events to text messages. Users can use a pre-defined text format where event conversion is carried out without any additional configurations, or use custom placeholder(using <code>{{</code> and <code>}}</code> or <code>{{{</code> and <code>}}}</code>) to map custom text messages. All variables are HTML escaped by default.<br>For example:<br><code>&</code> is replaced with <code>&amp;amp;</code><br><code>"</code> is replaced with <code>&amp;quot;</code><br><code>=</code> is replaced with <code>&amp;#61;</code><br>If you want to return unescaped HTML, use the triple mustache <code>{{{</code> instead of double <code>{{</code>.</p>

<span id="syntax" class="md-typeset" style="display: block; font-weight: bold;">Syntax</span>
```
@sink(..., @map(type="text", event.grouping.enabled="<BOOL>", delimiter="<STRING>", new.line.character="<STRING>")
```

<span id="query-parameters" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">QUERY PARAMETERS</span>
<table>
    <tr>
        <th>Name</th>
        <th style="min-width: 20em">Description</th>
        <th>Default Value</th>
        <th>Possible Data Types</th>
        <th>Optional</th>
        <th>Dynamic</th>
    </tr>
    <tr>
        <td style="vertical-align: top">event.grouping.enabled</td>
        <td style="vertical-align: top; word-wrap: break-word">If this parameter is set to <code>true</code>, events are grouped via a delimiter when multiple events are received. It is required to specify a value for the <code>delimiter</code> parameter when the value for this parameter is <code>true</code>.</td>
        <td style="vertical-align: top">false</td>
        <td style="vertical-align: top">BOOL</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">delimiter</td>
        <td style="vertical-align: top; word-wrap: break-word">This parameter specifies how events are separated when a grouped event is received. This must be a whole line and not a single character.</td>
        <td style="vertical-align: top">~~~~~~~~~~</td>
        <td style="vertical-align: top">STRING</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">new.line.character</td>
        <td style="vertical-align: top; word-wrap: break-word">This attribute indicates the new line character of the event that is expected to be received. This is used mostly when communication between 2 types of operating systems is expected. For example, Linux uses <code>\n</code> whereas Windows uses <code>\r\n</code> as the end of line character.</td>
        <td style="vertical-align: top">\n</td>
        <td style="vertical-align: top">STRING</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
</table>

<span id="examples" class="md-typeset" style="display: block; font-weight: bold;">Examples</span>
<span id="example-1" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">EXAMPLE 1</span>
```
@sink(type='inMemory', topic='stock', @map(type='text'))
define stream FooStream (symbol string, price float, volume long);
```
<p style="word-wrap: break-word">This query performs a default text input mapping. The expected output is as follows:<br>symbol:"WSO2",<br>price:55.6,<br>volume:100</p>

<span id="example-2" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">EXAMPLE 2</span>
```
@sink(type='inMemory', topic='stock', @map(type='text', event.grouping.enabled='true'))
define stream FooStream (symbol string, price float, volume long);
```
<p style="word-wrap: break-word">This query performs a default text input mapping with event grouping. The expected output is as follows:<br>symbol:"WSO2",<br>price:55.6,<br>volume:100<br>~~~~~~~~~~<br>symbol:"WSO2",<br>price:55.6,<br>volume:100</p>

<span id="example-3" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">EXAMPLE 3</span>
```
@sink(type='inMemory', topic='stock', @map(type='text',  @payload("SensorID : {{symbol}}/{{volume}}, SensorPrice : Rs{{price}}/=, Value : {{volume}}ml")))
define stream FooStream (symbol string, price float, volume long);
```
<p style="word-wrap: break-word">This query performs a custom text mapping. The expected output is as follows:<br>SensorID : wso2/100, SensorPrice : Rs1000/=, Value : 100ml <br>for the following siddhi event.<br>{wso2,1000,100}</p>

<span id="example-4" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">EXAMPLE 4</span>
```
@sink(type='inMemory', topic='stock', @map(type='text', event.grouping.enabled='true', @payload("Stock price of {{symbol}} is {{price}}")))
define stream FooStream (symbol string, price float, volume long);
```
<p style="word-wrap: break-word">This query performs a custom text mapping with event grouping. The expected output is as follows:<br>Stock price of WSO2 is 55.6<br>~~~~~~~~~~<br>Stock price of WSO2 is 55.6<br>~~~~~~~~~~<br>Stock price of WSO2 is 55.6<br>for the following siddhi event.<br>{WSO2,55.6,10}</p>

<span id="example-5" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">EXAMPLE 5</span>
```
@sink(type='inMemory', topic='stock', @map(type='text',  @payload("SensorID : {{{symbol}}}/{{{volume}}}, SensorPrice : Rs{{{price}}}/=, Value : {{{volume}}}ml")))
define stream FooStream (symbol string, price float, volume long);
```
<p style="word-wrap: break-word">This query performs a custom text mapping to return unescaped HTML. The expected output is as follows:<br>SensorID : a&b/100, SensorPrice : Rs1000/=, Value : 100ml <br>for the following siddhi event.<br>{a&b,1000,100}</p>

## Sourcemapper

### text *<a target="_blank" href="https://siddhi.io/en/v5.0/docs/query-guide/#source-mapper">(Source Mapper)</a>*

<p style="word-wrap: break-word">This extension is a text to Siddhi event input mapper. Transports that accept text messages can utilize this extension to convert the incoming text message to Siddhi event. Users can either use a pre-defined text format where event conversion happens without any additional configurations, or specify a regex to map a text message using custom configurations.</p>

<span id="syntax" class="md-typeset" style="display: block; font-weight: bold;">Syntax</span>
```
@source(..., @map(type="text", regex.groupid="<STRING>", fail.on.missing.attribute="<BOOL>", event.grouping.enabled="<BOOL>", delimiter="<STRING>", new.line.character="<STRING>")
```

<span id="query-parameters" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">QUERY PARAMETERS</span>
<table>
    <tr>
        <th>Name</th>
        <th style="min-width: 20em">Description</th>
        <th>Default Value</th>
        <th>Possible Data Types</th>
        <th>Optional</th>
        <th>Dynamic</th>
    </tr>
    <tr>
        <td style="vertical-align: top">regex.groupid</td>
        <td style="vertical-align: top; word-wrap: break-word">This parameter specifies a regular expression group. The <code>groupid</code> can be any capital letter (e.g., regex.A,regex.B .. etc). You can specify any number of regular expression groups. In the attribute annotation, you need to map all attributes to the regular expression group with the matching group index. If you need to to enable custom mapping, it is required to specifythe matching group for each and every attribute.</td>
        <td style="vertical-align: top"></td>
        <td style="vertical-align: top">STRING</td>
        <td style="vertical-align: top">No</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">fail.on.missing.attribute</td>
        <td style="vertical-align: top; word-wrap: break-word">This parameter specifies how unknown attributes should be handled. If it is set to <code>true</code> a message is dropped if its execution fails, or if one or more attributes do not have values. If this parameter is set to <code>false</code>, null values are assigned to attributes with missing values, and messages with such attributes are not dropped.</td>
        <td style="vertical-align: top">true</td>
        <td style="vertical-align: top">BOOL</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">event.grouping.enabled</td>
        <td style="vertical-align: top; word-wrap: break-word">This parameter specifies whether event grouping is enabled or not. To receive a group of events together and generate multiple events, this parameter must be set to <code>true</code>.</td>
        <td style="vertical-align: top">false</td>
        <td style="vertical-align: top">BOOL</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">delimiter</td>
        <td style="vertical-align: top; word-wrap: break-word">This parameter specifies how events must be separated when multiple events are received. This must be whole line and not a single character.</td>
        <td style="vertical-align: top">~~~~~~~~~~</td>
        <td style="vertical-align: top">STRING</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">new.line.character</td>
        <td style="vertical-align: top; word-wrap: break-word">This attribute indicates the new line character of the event that is expected to be received. This is used mostly when communication between 2 types of operating systems is expected. For example, Linux uses <code>\n</code> as the end of line character whereas windows uses <code>\r\n</code>.</td>
        <td style="vertical-align: top">\n</td>
        <td style="vertical-align: top">STRING</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
</table>

<span id="examples" class="md-typeset" style="display: block; font-weight: bold;">Examples</span>
<span id="example-1" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">EXAMPLE 1</span>
```
@source(type='inMemory', topic='stock', @map(type='text'))
define stream FooStream (symbol string, price float, volume long);
```
<p style="word-wrap: break-word">This query performs a default text input mapping. The expected input is as follows:<br>symbol:"WSO2",<br>price:55.6,<br>volume:100<br>OR<br>symbol:'WSO2',<br>price:55.6,<br>volume:100<br><br>If group events is enabled then input should be as follows: <br>symbol:"WSO2",<br>price:55.6,<br>volume:100<br>~~~~~~~~~~<br>symbol:"WSO2",<br>price:55.6,<br>volume:100</p>

<span id="example-2" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">EXAMPLE 2</span>
```
@source(type='inMemory', topic='stock', @map(type='text', fail.on.unknown.attribute = 'true', regex.A='(\w+)\s([-0-9]+)',regex.B='volume\s([-0-9]+)', @attributes(symbol = 'A[1]',price = 'A[2]',volume = 'B')))
define stream FooStream (symbol string, price float, volume long);
```
<p style="word-wrap: break-word">This query performs a custom text mapping. The expected input is as follows:<br>wos2 550 volume 100<br><br>If group events is enabled then input should be as follows: <br>wos2 550 volume 100<br>~~~~~~~~~~<br>wos2 550 volume 100<br>~~~~~~~~~~<br>wos2 550 volume 100<br></p>

