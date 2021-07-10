MongoDB Internals

Question III

1. Demonstrate the usage of $match, $group, aggregate pipelines.
	```
   db.Movie.aggregate([{ $match : { Genre : "Action"}}]).pretty()
	```
	```
   db.Movie.aggregate([{ $group : {_id : "$Genre", Avg_Rating : {$avg : "$IMDB Rating"}}}]).pretty()
	```
	```
	db.Movie.aggregate([{ $match: {Referable : true}}, { $group : {_id : "$Genre", Avg_Rating : {$avg : "$IMDB Rating"}}}]).pretty()
	```

2. Demonstrate the Map-Reduce aggregate function on this dataset.

   ```
   var mapper = function(){emit(this.Genre,this["Positive Feedback"])}
   var reduce = function(Genre, Feedback){return Array.sum(Feedback)}
   db.Movie.mapReduce(mapper, reduce, {out: "FeedbackOut"})
   db.FeedbackOut.find().pretty()
   ```

   

3. Count the number of Movies which belong to the thriller category and find out the total number of positive reviews in that category.

  ```
  db.Movie.aggregate([{$match : { Genre : "Thriller" }}, { $group : { _id : { Genre : "Thriller"}, count : { $sum : 1}, "Total Positive Reviews" : {$sum : "$Positive Feedback"}}}])
  ```

  

4. Group all the records by genre and find out the total number of positive feedbacks by genre.

   ```
   db.Movie.aggregate([{ $group : {_id : "$Genre", "Total Positive Reviews" : { $sum : "$Positive Feedback"}}}]).pretty()
   ```

   