mongoimport -d webxray -c germany --type=csv --file reports/german/network.csv --headerline 

```
{ "_id" : ObjectId("5cc998d7c8f724c6404eab70"), "Date" : "2019-05-01T12:55:02Z", "Page Domain" : "Bild.de", "3P Element Domain" : "facebook.com", "3P Domain Owner" : "Facebook", "3P Domain Owner Country" : "US" }
{ "_id" : ObjectId("5cc998d7c8f724c6404eab71"), "Date" : "2019-05-01T12:55:02Z", "Page Domain" : "Bild.de", "3P Element Domain" : "facebook.net", "3P Domain Owner" : "Facebook", "3P Domain Owner Country" : "US" }
{ "_id" : ObjectId("5cc998d7c8f724c6404eab72"), "Date" : "2019-05-01T12:55:02Z", "Page Domain" : "Bild.de", "3P Element Domain" : "google.com", "3P Domain Owner" : "Google", "3P Domain Owner Country" : "US" }
```
This query convert the data imported above, in our format


```
db.getCollection('germany').aggregate([
 { $group: { _id: "$Page Domain", trackers: { $addToSet: "$3P Domain Owner"}, count: { $sum: 1 }}},
 { $project: { site: "$_id", trackers: true, _id: 0 }}
])
```

Tasks to be done:
  * build a scripts which converts the 'Date' in ISODate()
