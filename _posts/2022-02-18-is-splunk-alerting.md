---
title: Is Splunk Alerting
author:
  name: SilentGlasses
  link: https://github.com/SilentGlasses
date: 2022-02-18 15:0:00
categories: [Observability, Splunk]
tags: [observability, splunk, administration]
---

I recently came cross an issue where Splunk stopped alerting, honestly, no idea why.. Still investigating. But in the interim I wanted to get some additional context for the investigation.  I will update this post once I have more details but I ended up building this dashboard that shows the past 7 days of alerting with some different contextual views with a timepicker defaulted to last 7 days:

* Total Alerts Sent (count)
* Alerts Sent (timechart)
* Alerts Sent by Host (timechart)
* Alerts Sent by App (timechart)
* Alerts Sent by Search (timechart)

```sql
<form theme="light">
  <label>Splunk Alerting Dashboard</label>
  <description>Show me the alerts</description>
  <fieldset submitButton="false">
    <input type="time" token="field1">
      <label></label>
      <default>
        <earliest>-7d@h</earliest>
        <latest>now</latest>
      </default>
    </input>
  </fieldset>
  <row>
    <panel id="TotalAlertsSent">
      <title>Total Alerts Sent</title>
      <html depends="$hiddenForCSS$">
        <style>
          #TotalAlertsSent{width: 20% !important;}
        </style>
      </html>
      <single>
        <search>
          <query>index=_internal sourcetype=splunkd component=sendmodalert | stats count</query>
          <earliest>$field1.earliest$</earliest>
          <latest>$field1.latest$</latest>
        </search>
        <option name="drilldown">none</option>
        <option name="refresh.display">progressbar</option>
      </single>
    </panel>
    <panel id="AlertsSentChart">
      <title>Alerts Sent</title>
      <html depends="$hiddenForCSS$">
        <style>
          #AlertsSentChart{width: 80% !important;}
        </style>
      </html>
      <chart>
        <search>
          <query>index=_internal sourcetype=splunkd component=sendmodalert | timechart count</query>
          <earliest>$field1.earliest$</earliest>
          <latest>$field1.latest$</latest>
        </search>
        <option name="charting.axisTitleX.visibility">collapsed</option>
        <option name="charting.axisTitleY.visibility">visible</option>
        <option name="charting.chart">line</option>
        <option name="charting.drilldown">none</option>
        <option name="charting.legend.placement">none</option>
        <option name="height">155</option>
        <option name="refresh.display">progressbar</option>
      </chart>
    </panel>
  </row>
  <row>
    <panel>
      <title>Alerts Sent by Host</title>
      <chart>
        <search>
          <query>index=_internal sourcetype=splunkd component=sendmodalert
  | timechart count by host</query>
          <earliest>$field1.earliest$</earliest>
          <latest>$field1.latest$</latest>
        </search>
        <option name="charting.chart">line</option>
        <option name="charting.drilldown">none</option>
        <option name="charting.legend.placement">bottom</option>
        <option name="refresh.display">progressbar</option>
      </chart>
    </panel>
  </row>
  <row>
    <panel>
      <title>Alerts Sent by App</title>
      <chart>
        <search>
          <query>index=_internal sourcetype=splunkd component=sendmodalert
  | where isnotnull(app)
  | timechart count by app</query>
          <earliest>$field1.earliest$</earliest>
          <latest>$field1.latest$</latest>
        </search>
        <option name="charting.chart">line</option>
        <option name="charting.drilldown">none</option>
        <option name="charting.legend.placement">bottom</option>
        <option name="refresh.display">progressbar</option>
      </chart>
    </panel>
  </row>
  <row>
    <panel>
      <title>Alerts Sent by Search</title>
      <chart>
        <search>
          <query>index=_internal sourcetype=splunkd component=sendmodalert
  | where isnotnull(search)
  | timechart count by search useother=0</query>
          <earliest>$field1.earliest$</earliest>
          <latest>$field1.latest$</latest>
        </search>
        <option name="charting.chart">line</option>
        <option name="charting.drilldown">none</option>
        <option name="refresh.display">progressbar</option>
      </chart>
    </panel>
  </row>
</form>
```
