{% if page.cloud == true %}
  {% capture link_prefix %}../{{site.versions["stable"]}}/{% endcapture %}
  {% assign page_prefix = "" %}
{% else %}
  {% assign link_prefix = "" %}
  {% assign page_prefix = "ui-" %}
{% endif %}

### Time interval

To view [statement fingerprints](#sql-statement-fingerprints) within a specific time interval, click the time interval selector and pick an interval. The time interval field supports present time intervals (1 Hour, 6 Hours, 1 Day, etc.) and custom time intervals. To select a custom time interval, click the time interval field and select **Custom time interval**. In the **Start (UTC)** and **End (UTC)** fields select or type a date and time.

Use the arrow keys to cycle through previous and next time intervals. When you select a time interval, the same interval is selected in the [Metrics]({{ link_prefix }}ui-overview.html#metrics) page.

It's possible to select an interval for which no statement statistics exist. CockroachDB persists statement statistics up to 1 million rows before the oldest row is deleted. The retention period of statistics is reduced the more active a workload is and the more distinct statement fingerprints there are.

### Filter

To filter the statements:

1. Click the **Filters** field.
      - To filter by [application]({{ link_prefix }}connection-parameters.html#additional-connection-parameters), select **App** and select one or more applications.

          - Queries from the SQL shell are displayed under the `$ cockroach` app.
          - If you haven't set `application_name` in a client connection string, it appears as `unset`.
      - To filter by one or more databases (**Database**), SQL statement types (**Statement Type**), or nodes on which the statement ran (**Node**), click the field and select one or more checkboxes.

          The **Statement Type** values map to the CockroachDB statement types [data definition language (DDL)]({{ link_prefix }}sql-statements.html#data-definition-statements), [data manipulation language (DML)]({{ link_prefix }}sql-statements.html#data-manipulation-statements), [data control language (DCL)]({{ link_prefix }}sql-statements.html#data-control-statements), and [transaction control language (TCL)]({{ link_prefix }}sql-statements.html#transaction-control-statements).
      - To display only statement fingerprints that take longer than a specified time to run, specify the time and units.
      - To display only statement fingerprints with queries that cause full table scans, click **Only show statements that contain queries with full table scans**.

1. Click **Apply**.

The following screenshot shows the statements that contain the string `rides` for the `movr` application:

<img src="{{ 'images/v22.1/movr-statements-rides.png' | relative_url }}" alt="Movr rides statements" style="border:1px solid #eee;max-width:100%" />
