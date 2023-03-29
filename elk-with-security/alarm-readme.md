To set up alerts in Elasticsearch/Kibana, you can use the Watcher feature, which allows you to create and manage alerts based on a set of conditions. Here's a basic example of how you can create an alert that triggers when the average CPU usage of your servers exceeds a certain threshold:

First, create an Elasticsearch query that retrieves the average CPU usage of your servers:




GET /_search
{
  "aggs": {
    "avg_cpu": {
      "avg": {
        "field": "system.cpu.total.pct"
      }
    }
  }
}






Next, create a Watcher alert that uses the above query and triggers when the average CPU usage is above a certain threshold:

PUT _watcher/watch/cpu_alert
{
  "trigger": {
    "schedule": {
      "interval": "5m"
    }
  },
  "input": {
    "search": {
      "request": {
        "indices": [
          "metricbeat-*"
        ],
        "body": {
          "query": {
            "match_all": {}
          },
          "aggs": {
            "avg_cpu": {
              "avg": {
                "field": "system.cpu.total.pct"
              }
            }
          }
        }
      }
    }
  },
  "condition": {
    "compare": {
      "ctx.payload.aggregations.avg_cpu.value": {
        "gt": 80
      }
    }
  },
  "actions": {
    "send_email": {
      "email": {
        "to": "youremail@example.com",
        "subject": "CPU usage alert!",
        "body": {
          "text": "The average CPU usage has exceeded the threshold of 80%."
        }
      }
    }
  }
}



In this example, the alert runs every 5 minutes and triggers when the average CPU usage is above 80%. When the alert triggers, it sends an email to youremail@example.com with the subject "CPU usage alert!" and the body "The average CPU usage has exceeded the threshold of 80%." You can also configure additional actions, such as sending a text message or executing a custom script.

