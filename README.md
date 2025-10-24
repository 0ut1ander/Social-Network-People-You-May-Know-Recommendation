# Social Network "People You May Know" Recommendation

A scalable friend recommendation engine for social networks using PySpark. This project analyzes user graph data and applies mutual friend counts and influence scoring to generate personalized "People You May Know" top 10 suggestions per user, optimized for large-scale data.

## Technical Overview

This project uses distributed graph analytics to suggest new friend connections for each user in a social network. It combines classic mutual friend counting with an influence-score algorithm to generate more relevant and diverse recommendations.

### Data Ingestion & Representation

- The network is represented as user-to-friends mappings.
- Data is loaded into Spark RDDs for scalable, in-memory processing.

### Mutual Friend Scoring

- For each user, all pairs of their friends are examined to count shared mutual friends.
- Candidate pairs exclude existing connections using broadcast variables.
- Scores are aggregated for each user pair and sorted to select top recommendations.

### Influence Scoring

- Recommendations are further refined by weighting each suggestion using the inverse of the recommending user's friend count (`1 / num_friends`). 
- This highlights less-connected users and creates more balanced and interesting friend suggestions.

### Distributed Spark Pipeline

- Multiple RDD transformations (`flatMap`, `reduceByKey`, `groupByKey`, `union`, etc.) are used throughout the workflow.
- Broadcast variables efficiently share existing connection and friend count metadata across workers.
- The pipeline ensures all users, even those with no friends, are included in the recommendation output.

### Output & Formatting

- Each user receives up to 10 personalized recommendations, ranked by score.
- Outputs are suitable for integration or downstream analysis.

## Rationale & Design Thoughts

- **Scalability:** Built atop Spark RDDs and broadcast variables for distributed performance on big data.
- **Accuracy & Relevance:** Adopts mutual friend logic (industry standard) plus influence scoring for diversity.
- **Extensibility:** Easily adaptable to add features (location, interaction frequency, etc.) or alternate ranking methods.
- **Optimization:** Caching and pipeline design help control resource usage in Spark.

## Limitations & Future Work

- Mutual friend candidate generation can be computationally intensive for very large networks.
- Future enhancements could include filtering, approximation techniques, or additional social/behavioral features.

## Notes

- The repo includes code for both mutual friend and influence scoring algorithms.
- Functions and Spark workflow are documented inline and in comments.

## License
