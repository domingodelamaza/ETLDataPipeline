# Verificar que todo el Docker container ejecutado esten en la misma network, en CMD Windows:
> docker run --rm --network document-streaming-main_default --name my-api-ingest -p 80:80 api-ingest

# Esto sirve para que Kafka reciba en su nodo consumidor información, ya que al parecer mi main.py no hacia "reload" 
Deployment Concepts¶
These examples run the server program (e.g Uvicorn), starting a single process, listening on all the IPs (0.0.0.0) on a predefined port (e.g. 80).

This is the basic idea. But you will probably want to take care of some additional things, like:

Security - HTTPS
Running on startup
Restarts
Replication (the number of processes running)
Memory
Previous steps before starting

> pip install "hypercorn[trio]"
> hypercorn main:app --worker-class trio


# MOngoDB y funcion para transformar string desde Kafka a MongoDB

#OPCION 1 STRING AS A JSON

#Sin funcion: y solo comando:
# Write the message into MongoDB
def foreach_batch_function(df, epoch_id):
    # Transform and write batchDF in this foreach

    # writes the dataframe with complete kafka message into mongodb
    df.write.format("com.mongodb.spark.sql.DefaultSource").mode("append").save()
    

{
    _id: ObjectId('62be052502ae380043263f31'),
    value: '{"InvoiceNo": 536365, "StockCode": "85123A", "Description": "WHITE HANGING HEART T-LIGHT HOLDER", "Quantity": 6, "InvoiceDate": "12-02-2010 08:26:00", "UnitPrice": 2.55, "CustomerID": 17850, "Country": "United Kingdom"}'
}

OPCION 2.

# Write the message into MongoDB
def foreach_batch_function(df, epoch_id):
    # Transform and write batchDF in this foreach

 # writes the dataframe with complete kafka message into mongodb
    df.write.format("com.mongodb.spark.sql.DefaultSource").mode("append").save()

# Start the MongoDB stream and wait for termination
df.writeStream.foreachBatch(foreach_batch_function).start().awaitTermination()

{
    _id: ObjectId('62be072b02ae380212c23498'),
    value: BinData(0, 'eyJJbnZvaWNlTm8iOiA1MzYzNjUsICJTdG9ja0NvZGUiOiAiODUxMjNBIiwgIkRlc2NyaXB0aW9uIjogIldISVRFIEhBTkdJTkcgSEVBUlQgVC1MSUdIVCBIT0xERVIiLCAiUXVhbnRpdHkiOiA2LCAiSW52b2ljZURhdGUiOiAiMTItMDItMjAxMCAwODoyNjowMCIsICJVbml0UHJpY2UiOiAyLjU1LCAiQ3VzdG9tZXJJRCI6IDE3ODUwLCAiQ291bnRyeSI6ICJVbml0ZWQgS2luZ2RvbSJ9'),
    topic: 'ingestion-topic',
    partition: 0,
    offset: 4,
    timestamp: ISODate('2022-06-30T20:27:18.881Z'),
    timestampType: 0
}

OPCION 3

# Write the message into MongoDB
def foreach_batch_function(df, epoch_id):
    # Transform and write batchDF in this foreach

#Transform the values of all rows in column value and create a dataframe out of it (will also only have one row)
    df2=df.withColumn("value",from_json(df.value,MapType(StringType(),StringType())))    
   
    # Transform the dataframe so that it will have individual columns 
    df3= df2.select(["value.Quantity","value.UnitPrice","value.Country","value.CustomerID","value.StockCode","value.Description","value.InvoiceDate","value.InvoiceNo"])
    
    # Send the dataframe into MongoDB which will create a BSON document out of it
    df3.write.format("com.mongodb.spark.sql.DefaultSource").mode("append").save()
    
    pass

# Start the MongoDB stream and wait for termination
df1.writeStream.foreachBatch(foreach_batch_function).start().awaitTermination()

{
    _id: ObjectId('62be092a02ae3803a31f0353'),
    Quantity: '6',
    UnitPrice: '2.55',
    Country: 'United Kingdom',
    CustomerID: '17850',
    StockCode: '85123A',
    Description: 'WHITE HANGING HEART T-LIGHT HOLDER',
    InvoiceDate: '12-02-2010 08:26:00',
    InvoiceNo: '536365'
}