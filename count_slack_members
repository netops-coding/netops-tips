require "json"
require "open-uri"
require 'influxdb'
require 'time'

## ここから取得可能 https://api.slack.com/methods/channels.list/test ##
json = open("https://slack.com/api/channels.list?token=xxxxxxxxxxxxxxxxxxxxxxxxxx&pretty=1").read
api_response = JSON.parse(json)
members = api_response["channels"][0]["num_members"]

## influxDBの情報
host = "x.x.x.x"
username = "xxxxxxx"
password = "xxxxxxx"
database = "xxxxxxx"
time_precision = "s"
time = Time.now

influxdb = InfluxDB::Client.new database, :username => username,
                                          :password => password,
                                          :host => host,
                                          :port => 443,
                                          :time_precision => time_precision,
                                          :verify_ssl => false,
                                          :use_ssl => true

netops_users = {}
netops_users.store("netops_member", members)

netops_users.each do |series, value|
  data = {
  :value => value,
  :time => time.to_i
  }
  influxdb.write_point(series, data)
end
