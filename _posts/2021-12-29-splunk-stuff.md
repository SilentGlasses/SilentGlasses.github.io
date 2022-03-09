---
title: General Splunk Things
author:
  name: SilentGlasses
  link: https://github.com/SilentGlasses
date: 2021-12-29 02:00:00
categories: [Observability, Splunk]
tags: [observability, splunk, administration]
---

This page will host general Splunk things that I find useful to reference and keep an eye on things in Splunk.

Splunk is a data mining tool that is geared for speedy indexing of high amounts of data. Using this data it specializes in being able to visualize this data in order to make sense of your logs. It captures, indexes and correlates near real-time machine data in a searchable repository from which you can generate graphs, reports, alerts, dashboards and more.

## Helpful Tidbits

* Enable Search assistant (Account -> Preferences -> SPL Editor -> set to Full)
* Specify the index, source, or source type
    * Take the time to learn the details of your data, which indexes contain them, the sources of your data, and the source types. Knowing this information about your data helps you narrow down your searches.
* Be Specific
    * Use the most specific terms in your search that you can. If possible, avoid using wildcard (`*`) characters.
* Avoid certain Conditions
    * More resources are used tracking `NOT` expressions than if you specify what you are looking for. Where ever possible, avoid using `NOT` expressions.
* **NEVER** use **All Time** or **Real Time**
    * These pull every available datapoint in Splunk and will cause extreme resource drain and possibly crash Splunk.
* Always limit time ranges to reduce system stress
    * One of the most effective ways to limit the data that is pulled off from disk is to limit the time range. Be as specific with the time as you can to only pull the pertinent data.
* Read the Creating Optimized Splunk Searches article. _(future post)_
    * Here you will find lots of good information on building searches.
* Check the Job Inspector
    * This is a tool that allows you to examine your stats of your searches including where Splunk is spending it's time. Use it to troubleshoot performance and understand the impact on processing.
* Use proper order of commands
    * The aim here is to not overburden the search head by making the indexer(s) do some of the work. Move commands that bring data to the search head as late as possible in your search criteria.
    * Commands are chained together with a pipe (`|`) character to indicate that the output of one command feeds into the next command on the right. Add each chain on a new line for easy readability.

## Splunk Queries and Dashboards

### 30 Day License Used vs Total (RolloverSummary)

```
index=_internal source=*license_usage.log type="RolloverSummary" earliest=-30d@d
| eval _time=_time - 43200
| bin _time span=1d
| stats latest(b) AS b by slave, pool, _time
| timechart span=1d sum(b) AS "Used" fixedrange=false
| fields - _timediff
| foreach * [eval <<FIELD>>=round('<<FIELD>>'/1024/1024/1024, 3)]
| eval Available = 900
```

### Usage by Host - Top 10 over Last 7 Days (Non Splunk)

```
index=_internal source="*metrics.log" group=per_host_thruput host!="splunk*"
  | eval GB=round((kb/1024/1024),2)
  | chart sum(GB) AS GB by host
  | sort 10 -GB
```

### Usage by Host - Top 10 over last 30 days (Non Splunk)

```
index=_internal source="*metrics.log" group=per_host_thruput host!="splunk*"
  | eval GB=round((kb/1024/1024),2)
  | chart sum(GB) AS GB by host
  | sort 10 -GB
```

### Usage by Host - Top 10 over last 7 days (Splunk)

```
index=_internal source="*metrics.log" group=per_index_thruput host="splunk*"
  | eval GB=round((kb/1024/1024),2)
  | chart sum(GB) AS GB by host
  | sort 10 -GB
```

### Usage by Host - Top 10 over last 30 days (Splunk)

```
index=_internal source="*metrics.log" group=per_index_thruput host="splunk*"
  | eval GB=round((kb/1024/1024),2)
  | chart sum(GB) AS GB by host
  | sort 10 -GB
```

### Usage by Index - Top 10 over Last 7 Days

```
index=_internal source=*license_usage.log type=Usage
  | eval GB=round((b/1024/1024/1024),2)
  | chart sum(GB) AS GB by idx
  | sort 10 -GB
```

### Usage by Index - Top 10 over Last 30 Days

```
index=_internal source=*license_usage.log type=Usage
  | eval GB=round((b/1024/1024/1024),2)
  | chart sum(GB) AS GB by idx
  | sort 10 -GB
```

### Usage by input - Top 10 over last 7 days (Non _*)

```
index=_internal source="*metrics.log" group=per_source_thruput series!="_*" series!="*splunk*metrics.log"
  | eval GB=round((kb/1024/1024),2)
  | chart sum(GB) AS GB by series
  | sort 10 -GB
```

### Usage by input - Top 10 over last 30 days (Non _*)

```
index=_internal source="*metrics.log" group=per_source_thruput series!="_*" series!="*splunk*metrics.log"
  | eval GB=round((kb/1024/1024),2)
  | chart sum(GB) AS GB by series
  | sort 10 -GB
```

### Usage by Log File - Top 10 over last 7 days

```
index::_internal metrics NOT debug NOT sourcetype::splunk_web_access group="per_source_thruput" (series=/var/log*)
  | eval GB=round((kb/1024/1024),2)
  | chart sum(GB) AS GB by series
  | sort 10 -GB
```

### Usage by Log File - Top 10 over last 30 days

```
index::_internal metrics NOT debug NOT sourcetype::splunk_web_access group="per_source_thruput" (series=/var/log*)
  | eval GB=round((kb/1024/1024),2)
  | chart sum(GB) AS GB by series
  | sort 10 -GB
```

### User Last Login (Last 30 Days)

```
index=_audit action="login attempt" user NOT admin
  | stats max(timestamp) by user
  | rename max(timestamp) AS "Last Login"
  | sort -"Last Login"
```

### Top 10 Users in Last 30 Days

```
index=_audit action="login attempt" user NOT admin
  | stats count by user
  | sort -count
  | head 10
```

### License Usage by Index Dashboard

```
<form>
  <label>License Usage by Index</label>
  <description>License Usage by Index - Last 7 Days. Use this to determine when the Index started breaching License Usage</description>
  <fieldset submitButton="false">
    <input type="dropdown" token="idx" searchWhenChanged="true">
      <label>Index</label>
      <fieldForLabel>idx</fieldForLabel>
      <fieldForValue>idx</fieldForValue>
      <search>
        <query>index=_internal source=*license_usage.log* type=Usage NOT idx=sos | dedup idx | table idx | sort idx asc</query>
        <earliest>-7d@h</earliest>
        <latest>now</latest>
      </search>
    </input>
  </fieldset>
  <row>
    <panel>
      <title>Select an Index from the drop down above to populate this panel.</title>
      <chart>
        <search>
          <query>index="_internal" host=splunkidx* source="*metrics.log" earliest=-7d  series=$idx$
  | eval GB=kb / 1024 / 1024
  | timechart span=1d limit=0 sum(GB) by series</query>
          <earliest>-7d@h</earliest>
          <latest>now</latest>
        </search>
        <option name="charting.chart">column</option>
        <option name="charting.drilldown">none</option>
        <option name="refresh.display">progressbar</option>
      </chart>
    </panel>
  </row>
</form>
```

### Splunk Data Volume Investigation by Host

```
<form>
  <label>Splunk Data Volume Investigation by Host</label>
  <description>License Usage by Host - Last 7 Days. Use this to determine what on the host is breaching License Usage</description>
  <fieldset submitButton="false">
    <input type="dropdown" token="series" searchWhenChanged="true">
      <label>Hostname</label>
      <search>
        <query>index="_internal" source="*metrics.log" group="per_host_thruput"
  | dedup series
  | sort series asc
  | table series</query>
        <earliest>-7d@h</earliest>
        <latest>now</latest>
      </search>
      <fieldForLabel>series</fieldForLabel>
      <fieldForValue>series</fieldForValue>
    </input>
  </fieldset>
  <row>
    <panel>
      <title>KB/min (Last 2 hours)</title>
      <chart>
        <search>
          <query>index="_internal" source="*metrics.log" group="per_host_thruput" series="$series$"
  | eval MB=round((kb/1024),2)
  | timechart span=1m sum(MB) AS "Total MB"</query>
          <earliest>-2h@h</earliest>
          <latest>now</latest>
        </search>
        <option name="charting.axisTitleX.visibility">collapsed</option>
        <option name="charting.chart">line</option>
        <option name="charting.drilldown">none</option>
        <option name="charting.legend.placement">bottom</option>
      </chart>
    </panel>
  </row>
  <row>
    <panel>
      <title>MB/hr Comparison (Last 24 hours)</title>
      <chart>
        <search>
          <query>index="_internal" source="*metrics.log" group="per_host_thruput" earliest=-24h latest=now series=$series$
  | bin _time span=1h
  | stats sum(kb) AS kb_today by _time
  | join _time [ search index="_internal" source="*metrics.log" group="per_host_thruput" earliest=-168h latest=-144h series=$series$
  | bin _time span=1h
  | eval _time=(_time + 518400)
  | rename host AS series
  | stats sum(kb) AS kb_week_ago by _time ]
  | eval Mb_today=round((kb_today/1024),1), Mb_week_ago=round((kb_week_ago/1024),1), Mb_diff=abs(Mb_today - Mb_week_ago)
  | timechart max(Mb_today) AS "MB/hr (last 24 hours)" max(Mb_week_ago) AS "MB/hr (1 week ago)" max(Mb_diff) AS "MB/hr difference" span=1h</query>
          <earliest>-24h@h</earliest>
          <latest>now</latest>
        </search>
        <option name="charting.axisLabelsX.majorLabelStyle.overflowMode">ellipsisNone</option>
        <option name="charting.axisLabelsX.majorLabelStyle.rotation">0</option>
        <option name="charting.axisTitleX.visibility">collapsed</option>
        <option name="charting.axisTitleY.visibility">visible</option>
        <option name="charting.axisTitleY2.visibility">visible</option>
        <option name="charting.axisX.scale">linear</option>
        <option name="charting.axisY.minimumNumber">0</option>
        <option name="charting.axisY.scale">linear</option>
        <option name="charting.axisY2.enabled">0</option>
        <option name="charting.axisY2.scale">inherit</option>
        <option name="charting.chart">column</option>
        <option name="charting.chart.bubbleMaximumSize">50</option>
        <option name="charting.chart.bubbleMinimumSize">10</option>
        <option name="charting.chart.bubbleSizeBy">area</option>
        <option name="charting.chart.nullValueMode">zero</option>
        <option name="charting.chart.overlayFields">"MB/hr difference"</option>
        <option name="charting.chart.showDataLabels">none</option>
        <option name="charting.chart.sliceCollapsingThreshold">0.01</option>
        <option name="charting.chart.stackMode">default</option>
        <option name="charting.chart.style">shiny</option>
        <option name="charting.drilldown">all</option>
        <option name="charting.layout.splitSeries">0</option>
        <option name="charting.layout.splitSeries.allowIndependentYRanges">0</option>
        <option name="charting.legend.labelStyle.overflowMode">ellipsisMiddle</option>
        <option name="charting.legend.placement">bottom</option>
        <option name="charting.fieldColors">{"MB/hr (last 24 hours)": 0xC7D8C6, "MB/hr (1 week ago)": 0xEFD9C1, "MB/hr difference":0xAF473C}</option>
      </chart>
    </panel>
  </row>
  <row>
    <panel>
      <title>Data Points/hr (Last 24 hours)</title>
      <chart>
        <search>
          <query>index!="_internal" earliest=-24h latest=now host=$series$
  | bin _time span=1h
  | eval time_and_source=(_time . ":" . source)
  | stats count AS count_today by time_and_source
  | rex field=time_and_source "(?&lt;_time&gt;\d+):(?&lt;source&gt;.+$)"
  | timechart max(count_today) AS "Current Data Points" BY source span=1h useother=f</query>
          <earliest>-24h@h</earliest>
          <latest>now</latest>
        </search>
        <option name="charting.axisLabelsX.majorLabelStyle.overflowMode">ellipsisNone</option>
        <option name="charting.axisLabelsX.majorLabelStyle.rotation">0</option>
        <option name="charting.axisLabelsY2.majorUnit">5</option>
        <option name="charting.axisTitleX.text">/Current Data Points</option>
        <option name="charting.axisTitleX.visibility">collapsed</option>
        <option name="charting.axisTitleY.text">Data Points/hr</option>
        <option name="charting.axisTitleY.visibility">visible</option>
        <option name="charting.axisTitleY2.text"># of Sources</option>
        <option name="charting.axisTitleY2.visibility">visible</option>
        <option name="charting.axisX.scale">linear</option>
        <option name="charting.axisY.minimumNumber">0</option>
        <option name="charting.axisY.scale">linear</option>
        <option name="charting.axisY2.enabled">1</option>
        <option name="charting.axisY2.minimumNumber">0</option>
        <option name="charting.axisY2.scale">linear</option>
        <option name="charting.chart">column</option>
        <option name="charting.chart.bubbleMaximumSize">50</option>
        <option name="charting.chart.bubbleMinimumSize">10</option>
        <option name="charting.chart.bubbleSizeBy">area</option>
        <option name="charting.chart.nullValueMode">connect</option>
        <option name="charting.chart.overlayFields">"Number of sources (last 24 hours)","Number of sources (1 week ago)"</option>
        <option name="charting.chart.showDataLabels">none</option>
        <option name="charting.chart.sliceCollapsingThreshold">0.01</option>
        <option name="charting.chart.stackMode">stacked</option>
        <option name="charting.chart.style">shiny</option>
        <option name="charting.drilldown">none</option>
        <option name="charting.layout.splitSeries">0</option>
        <option name="charting.layout.splitSeries.allowIndependentYRanges">0</option>
        <option name="charting.legend.labelStyle.overflowMode">ellipsisStart</option>
        <option name="charting.legend.placement">bottom</option>
      </chart>
    </panel>
    <panel>
      <title>Data Points/hr (1 week ago)</title>
      <chart>
        <search>
          <query>index!="_internal" earliest=-168h latest=-144h host=$series$
  | bin _time span=1h
  | eval time_and_source=(_time . ":" . source)
  | stats count AS count_previous by time_and_source
  | rex field=time_and_source "(?&lt;_time&gt;\d+):(?&lt;source&gt;.+$)"
  | timechart max(count_previous) AS "Data Points (1 week ago)" BY source span=1h useother=f</query>
          <earliest>-24h@h</earliest>
          <latest>now</latest>
        </search>
        <option name="charting.axisLabelsX.majorLabelStyle.overflowMode">ellipsisNone</option>
        <option name="charting.axisLabelsX.majorLabelStyle.rotation">0</option>
        <option name="charting.axisTitleX.visibility">collapsed</option>
        <option name="charting.axisTitleY.text">Data Points/hr</option>
        <option name="charting.axisTitleY.visibility">visible</option>
        <option name="charting.axisTitleY2.visibility">visible</option>
        <option name="charting.axisX.scale">linear</option>
        <option name="charting.axisY.minimumNumber">0</option>
        <option name="charting.axisY.scale">linear</option>
        <option name="charting.axisY2.enabled">0</option>
        <option name="charting.axisY2.scale">inherit</option>
        <option name="charting.chart">column</option>
        <option name="charting.chart.bubbleMaximumSize">50</option>
        <option name="charting.chart.bubbleMinimumSize">10</option>
        <option name="charting.chart.bubbleSizeBy">area</option>
        <option name="charting.chart.nullValueMode">gaps</option>
        <option name="charting.chart.showDataLabels">none</option>
        <option name="charting.chart.sliceCollapsingThreshold">0.01</option>
        <option name="charting.chart.stackMode">stacked</option>
        <option name="charting.chart.style">shiny</option>
        <option name="charting.drilldown">none</option>
        <option name="charting.layout.splitSeries">0</option>
        <option name="charting.layout.splitSeries.allowIndependentYRanges">0</option>
        <option name="charting.legend.labelStyle.overflowMode">ellipsisStart</option>
        <option name="charting.legend.placement">bottom</option>
      </chart>
    </panel>
  </row>
  <row>
    <panel>
      <title>Data Points/day (Last 7 days)</title>
      <chart>
        <search>
          <query>index!="_internal" earliest=-7d latest=now host=$series$
  | bin _time span=1d
  | eval time_and_source=(_time . ":" . source)
  | stats count AS count_today by time_and_source
  | rex field=time_and_source "(?&lt;_time&gt;\d+):(?&lt;source&gt;.+$)"
  | timechart max(count_today) AS "Current Data Points" BY source span=1d useother=f</query>
          <earliest>-4h@m</earliest>
          <latest>now</latest>
        </search>
        <option name="charting.axisTitleX.visibility">collapsed</option>
        <option name="charting.chart">column</option>
        <option name="charting.chart.showDataLabels">none</option>
        <option name="charting.chart.stackMode">stacked</option>
        <option name="charting.drilldown">none</option>
        <option name="charting.layout.splitSeries">0</option>
        <option name="charting.legend.placement">bottom</option>
      </chart>
    </panel>
  </row>
  <row>
    <panel>
      <title>Data Points/day (Last 30 days)</title>
      <chart>
        <search>
          <query>index!="_internal" earliest=-30d latest=now host=$series$
  | bin _time span=1d
  | eval time_and_source=(_time . ":" . source)
  | stats count AS count_today by time_and_source
  | rex field=time_and_source "(?&lt;_time&gt;\d+):(?&lt;source&gt;.+$)"
  | timechart max(count_today) AS "Current Data Points" BY source span=1d useother=f</query>
          <earliest>$earliest$</earliest>
          <latest>$latest$</latest>
        </search>
        <option name="charting.axisTitleX.visibility">collapsed</option>
        <option name="charting.chart">column</option>
        <option name="charting.chart.stackMode">stacked</option>
        <option name="charting.drilldown">none</option>
        <option name="charting.legend.placement">bottom</option>
      </chart>
    </panel>
  </row>
</form>
```

### Splunk Data Volume Investigation by Source

```
<form>
  <label>Splunk Data Volume Investigation by Source</label>
  <description>License Usage by Source - Last 7 Days. Use this to determine where the source is breaching License Usage</description>
  <fieldset submitButton="false">
    <input type="dropdown" token="series" searchWhenChanged="true">
      <label>Source</label>
      <search>
        <query>index="_internal" source="*metrics.log" group="per_source_thruput" | dedup series | sort series asc | table series</query>
        <earliest>-7d@h</earliest>
        <latest>now</latest>
      </search>
      <fieldForLabel>series</fieldForLabel>
      <fieldForValue>series</fieldForValue>
    </input>
  </fieldset>
  <row>
    <panel>
      <title>KB/min (Last 2 hours)</title>
      <chart>
        <search>
          <query>index="_internal" source="*metrics.log" group="per_source_thruput" series="$series$"
  | eval MB=round((kb/1024),2)
  | timechart span=1m sum(MB) AS "Total MB"</query>
          <earliest>-2h@h</earliest>
          <latest>now</latest>
        </search>
        <option name="charting.chart">line</option>
        <option name="charting.drilldown">none</option>
        <option name="charting.legend.placement">bottom</option>
      </chart>
    </panel>
  </row>
  <row>
    <panel>
      <chart>
        <title>MB/hr Comparison</title>
        <search>
          <query>index="_internal" source="*metrics.log" group="per_source_thruput" earliest=-24h latest=now series=$series$
 | bin _time span=1h
 | stats sum(kb) AS kb_today by _time
 | join _time [ search index="_internal" source="*metrics.log" group="per_source_thruput" earliest=-168h latest=-144h series=$series$
 | bin _time span=1h
 | eval _time=(_time + 518400)
 | rename host AS series
 | stats sum(kb) AS kb_week_ago by _time ]
 | eval Mb_today=round((kb_today/1024),1), Mb_week_ago=round((kb_week_ago/1024),1), Mb_diff=abs(Mb_today - Mb_week_ago)
 | timechart max(Mb_today) AS "MB/hr (last 24 hours)" max(Mb_week_ago) AS "MB/hr (1 week ago)" max(Mb_diff) AS "MB/hr difference" span=1h</query>
          <earliest>-24h@h</earliest>
          <latest>now</latest>
        </search>
        <option name="charting.axisLabelsX.majorLabelStyle.overflowMode">ellipsisNone</option>
        <option name="charting.axisLabelsX.majorLabelStyle.rotation">0</option>
        <option name="charting.axisTitleX.visibility">collapsed</option>
        <option name="charting.axisTitleY.visibility">visible</option>
        <option name="charting.axisTitleY2.visibility">visible</option>
        <option name="charting.axisX.scale">linear</option>
        <option name="charting.axisY.minimumNumber">0</option>
        <option name="charting.axisY.scale">linear</option>
        <option name="charting.axisY2.enabled">0</option>
        <option name="charting.axisY2.scale">inherit</option>
        <option name="charting.chart">column</option>
        <option name="charting.chart.bubbleMaximumSize">50</option>
        <option name="charting.chart.bubbleMinimumSize">10</option>
        <option name="charting.chart.bubbleSizeBy">area</option>
        <option name="charting.chart.nullValueMode">zero</option>
        <option name="charting.chart.overlayFields">"MB/hr difference"</option>
        <option name="charting.chart.showDataLabels">none</option>
        <option name="charting.chart.sliceCollapsingThreshold">0.01</option>
        <option name="charting.chart.stackMode">default</option>
        <option name="charting.chart.style">shiny</option>
        <option name="charting.drilldown">all</option>
        <option name="charting.layout.splitSeries">0</option>
        <option name="charting.layout.splitSeries.allowIndependentYRanges">0</option>
        <option name="charting.legend.labelStyle.overflowMode">ellipsisMiddle</option>
        <option name="charting.legend.placement">bottom</option>
      </chart>
    </panel>
  </row>
  <row>
    <panel>
      <chart>
        <title>Data Points/hr (Last 24 hours) by Host</title>
        <search>
          <query>index!="_internal" earliest=-24h latest=now source=$series$
| bin _time span=1h | eval time_and_host=(_time . ":" . host)
| stats count AS count_today by time_and_host
| rex field=time_and_host "(?&lt;_time&gt;\d+):(?&lt;host&gt;.+$)"
| timechart max(count_today) AS "Current Data Points" BY host span=1h useother=f</query>
          <earliest>-24h@h</earliest>
          <latest>now</latest>
        </search>
        <option name="charting.axisLabelsX.majorLabelStyle.overflowMode">ellipsisNone</option>
        <option name="charting.axisLabelsX.majorLabelStyle.rotation">0</option>
        <option name="charting.axisLabelsY2.majorUnit">5</option>
        <option name="charting.axisTitleX.visibility">collapsed</option>
        <option name="charting.axisTitleY.text">Data Points/hr</option>
        <option name="charting.axisTitleY.visibility">visible</option>
        <option name="charting.axisTitleY2.text"># of Sources</option>
        <option name="charting.axisTitleY2.visibility">visible</option>
        <option name="charting.axisX.scale">linear</option>
        <option name="charting.axisY.minimumNumber">0</option>
        <option name="charting.axisY.scale">linear</option>
        <option name="charting.axisY2.enabled">1</option>
        <option name="charting.axisY2.minimumNumber">0</option>
        <option name="charting.axisY2.scale">linear</option>
        <option name="charting.chart">column</option>
        <option name="charting.chart.bubbleMaximumSize">50</option>
        <option name="charting.chart.bubbleMinimumSize">10</option>
        <option name="charting.chart.bubbleSizeBy">area</option>
        <option name="charting.chart.nullValueMode">connect</option>
        <option name="charting.chart.overlayFields">"Number of sources (last 24 hours)","Number of sources (1 week ago)"</option>
        <option name="charting.chart.showDataLabels">none</option>
        <option name="charting.chart.sliceCollapsingThreshold">0.01</option>
        <option name="charting.chart.stackMode">stacked</option>
        <option name="charting.chart.style">shiny</option>
        <option name="charting.drilldown">none</option>
        <option name="charting.layout.splitSeries">0</option>
        <option name="charting.layout.splitSeries.allowIndependentYRanges">0</option>
        <option name="charting.legend.labelStyle.overflowMode">ellipsisStart</option>
        <option name="charting.legend.placement">bottom</option>
      </chart>
    </panel>
    <panel>
      <chart>
        <title>Data Points/hr (1 week ago)</title>
        <search>
          <query>index!="_internal" earliest=-168h latest=-144h source=$series$
| bin _time span=1h | eval time_and_host=(_time . ":" . host)
| stats count AS count_today by time_and_host
| rex field=time_and_host "(?&lt;_time&gt;\d+):(?&lt;host&gt;.+$)"
| timechart max(count_today) AS "Current Data Points" BY host span=1h useother=f</query>
          <earliest>-24h@h</earliest>
          <latest>now</latest>
        </search>
        <option name="charting.axisLabelsX.majorLabelStyle.overflowMode">ellipsisNone</option>
        <option name="charting.axisLabelsX.majorLabelStyle.rotation">0</option>
        <option name="charting.axisTitleX.visibility">collapsed</option>
        <option name="charting.axisTitleY.visibility">visible</option>
        <option name="charting.axisTitleY2.visibility">visible</option>
        <option name="charting.axisX.scale">linear</option>
        <option name="charting.axisY.scale">linear</option>
        <option name="charting.axisY2.enabled">0</option>
        <option name="charting.axisY2.scale">inherit</option>
        <option name="charting.chart">column</option>
        <option name="charting.chart.bubbleMaximumSize">50</option>
        <option name="charting.chart.bubbleMinimumSize">10</option>
        <option name="charting.chart.bubbleSizeBy">area</option>
        <option name="charting.chart.nullValueMode">gaps</option>
        <option name="charting.chart.showDataLabels">none</option>
        <option name="charting.chart.sliceCollapsingThreshold">0.01</option>
        <option name="charting.chart.stackMode">stacked</option>
        <option name="charting.chart.style">shiny</option>
        <option name="charting.drilldown">none</option>
        <option name="charting.layout.splitSeries">0</option>
        <option name="charting.layout.splitSeries.allowIndependentYRanges">0</option>
        <option name="charting.legend.labelStyle.overflowMode">ellipsisStart</option>
        <option name="charting.legend.placement">bottom</option>
        <option name="charting.axisY.minimumNumber">0</option>
        <option name="charting.axisTitleY.text">Data Points/hr</option>
      </chart>
    </panel>
  </row>
</form>
```
