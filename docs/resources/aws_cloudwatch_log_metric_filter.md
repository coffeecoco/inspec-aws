---
title: About the aws_cloudwatch_log_metric_filter Resource
---

# aws_cloudwatch_log_metric_filter

Use the `aws_cloudwatch_log_metric_filter` InSpec audit resource to search for and test properties of individual AWS Cloudwatch Log Metric Filters.

A Log Metric Filter (LMF) is an AWS resource that observes log traffic, looks for a specified pattern, and updates a metric about the number times the match occurs.  The metric can also be connected to AWS Cloudwatch Alarms, so that actions can be taken when a match occurs.

<br>

## Syntax

An `aws_cloudwatch_log_metric_filter` resource block searches for an LMF, specified by several search options.  If more than one log metric filter matches, an error occurs.

    # Look for a LMF by its filter name and log group name.  This combination
    # will always either find at most one LMF - no duplicates.
    describe aws_cloudwatch_log_metric_filter(
      filter_name: 'my-filter',
      log_group_name: 'my-log-group'
    ) do
      it { should exist }
    end

    # Search for an LMF by pattern and log group.
    # This could result in an error if the results are not unique.
    describe aws_cloudwatch_log_metric_filter(
      log_group_name:  'my-log-group',
      pattern: 'my-filter'
    ) do
      it { should exist }
    end

<br>

## Resource Parameters

### filter_name

This is the identifier of the log metric filter within its log group.  To ensure you have a unique result, you must also provide the log_group_name.

    describe aws_cloudwatch_log_metric_filter(
      filter_name: 'my-filter'
    ) do
      it { should exist }
    end

### log_group_name

The name of the Cloudwatch Log Group that the LMF is watching.  Together with filter_name, this uniquely identifies an LMF.

    describe aws_cloudwatch_log_metric_filter(
      log_group_name: 'my-log-group',
    ) do
      it { should exist }
    end

### pattern

The filter pattern used to match entries from the logs in the log group.

    describe aws_cloudwatch_log_metric_filter(
      pattern: '"ERROR" - "Exiting"',
    ) do
      it { should exist }
    end

## Matchers

### exist

Matches (i.e., passes the test) if the resource parameters (search criteria) were able to locate exactly one LMF.

    describe aws_cloudwatch_log_metric_filter(
      log_group_name: 'my-log-group',
    ) do
      it { should exist }
    end

## Properties

### filter_name

The name of the LMF within the log_group.

    # Check the name of the LMF that has a certain pattern
    describe aws_cloudwatch_log_metric_filter(
      log_group_name: 'app-log-group',
      pattern: 'KERBLEWIE',
    ) do
      its('filter_name') { should cmp 'kaboom_lmf' }
    end

### log_group_name

The name of the log group that the LMF is watching.

    # Check which log group the LMF 'error-watcher' is watching 
    describe aws_cloudwatch_log_metric_filter(
      filter_name: 'error-watcher',
    ) do
      its('log_group_name') { should cmp 'app-log-group' }
    end

### metric_name, metric_namespace

The name and namespace of the Cloudwatch Metric that will be updated when the LMF matches.  You also need the `metric_namespace` to uniquely identify the metric.

    # Ensure that the LMF has the right metric name
    describe aws_cloudwatch_log_metric_filter(
      filter_name: 'my-filter',
      log_group_name: 'my-log-group',
    ) do
      its('metric_name') { should cmp 'MyMetric' }
      its('metric_namespace') { should cmp 'MyFantasticMetrics' }
    end

### pattern

The pattern used to match entries from the logs in the log group.

    # Ensure that the LMF is watching for errors
    describe aws_cloudwatch_log_metric_filter(
      filter_name: 'error-watcher',
      log_group_name: 'app-log-group',
    ) do
      its('pattern') { should cmp 'ERROR' }
    end

