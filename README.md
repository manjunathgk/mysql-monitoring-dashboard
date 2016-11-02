# mysql-monitoring-dashboard
MySQL Monitoring dashboard using collectd,influxdb and grafana

MySQL CollectD Plugin is based On:
https://github.com/chrisboulton/collectd-python-mysql AND 
https://github.com/isartmontane/collectd-python-mysql

MySQL Grafana dashboard is based on: 
https://grafana.net/dashboards/561
and 
https://grafana.net/dashboards/564


## MySQL Grafana dashboard 
Grafana dashboard is for basic startup monitoring.

## MySQL CollectD Plugin
A python MySQL plugin for CollectD. Designed for MySQL 5.5+, and specifically variants such as MariaDB or Percona Server.

Pulls most of the same metrics as the Percona Monitoring Plugins for Cacti. Collects over 350 MySQL metrics per interval.

Most MySQL monitoring plugins fetch a lot of information by parsing the output of `SHOW ENGINE INNODB STATUS`. This plugin prefers InnoDB statistics from `SHOW GLOBAL STATUS`. Percona Server and MariaDB provide most of these InnoDB metrics on `SHOW GLOBAL STATUS`.

Requires the Python MySQLdb package. (`python-mysqldb` on Debian)

## Installation
1. Place mysql.py in your CollectD python plugins directory
2. Configure the plugin in CollectD
3. Restart CollectD

## Configuration
If you don’t already have the Python module loaded, you need to configure it first:

    <LoadPlugin python>
    	Globals true
    </LoadPlugin>
    <Plugin python>
    	ModulePath "/path/to/python/modules"
    </Plugin>

You should then configure the MySQL plugin:

	<Plugin python>
		Import mysql
		<Module mysql>
			Host "localhost" (default: localhost)
			Port 3306 (default: 3306)
			User "root" (default: root)
			Password "xxxx" (default: empty)
			HeartbeatTable "percona.heartbeat" (if using pt-heartbeat to track slave lag)
			Verbose false (default: false)
		</Module>
	</Python>
 

## For more info collected MySQL Metrics and Statuses, please see https://github.com/chrisboulton/collectd-python-mysql 

 
### Query Response Times

For versions of MySQL with support for it and where enabled, `INFORMATION_SCHEMA.QUERY_RESPONSE_TIME` will be queried for metrics to generate a histogram of query response times.

[Additional information on response time histograms in Percona Server](http://www.percona.com/blog/2010/07/11/query-response-time-histogram-new-feature-in-percona-server/)

    response_time_total.1
    response_time_count.1
    ...
    response_time_total.14
    response_time_count.14

### Added support for PERFORMANCE_SCHEMA metrics
If Performance_schema is enabled you will get the following metrics as well.

    Number of connections per Account (host-user) - Total and current
    Number of connections per User - Total and current
    Number of connections per Host - Total and current
    Number of rows read per index - schema, table, index name, rows read
    Indexes not being used (didn't get any read) - schema, table, index_name
    Queries that raised errors/warnings - Query, number of executions, errors, warnings
    Slow queries - Query, number of executions, execution time (total,max,avg), rows sent (total, avg), scanned rows
    Added slow queries excluding table names. Very useful when you have different table with same 'schema'.

## License
MIT (http://www.opensource.org/licenses/mit-license.php)


