# Cypher direction validation competition
This repository that hold the information about the competition about validating and fixing relationship direction in Cypher statements based on the given schema.

## Motivation for the competition

In the time of LLMs, it is becoming increasingly popular to implement _Retrieval-Augmented Generation_ (RAG) applications, where you feed additional information to an LLM at query time to increase its ability to generate valid, accurate, and up-to-date answers.
This new RAG paradigm is bringing in the revolution in data accessibility, as it means that anybody, even non-technical users, can now ask questions about information stored in the database without requiring them to learn a database query language.
For example, when you want the users to be able to ask questions about the information stored in Neo4j, a graph database, you need to implement a so-called _text2cypher_ module that takes natural language as input and produces Cypher statements as output.
State-of-the-art LLMs are pretty good at generating Cypher statements.
However, most of them share a common flaw: they can sometimes mess up the direction of the relationship in the generated Cypher statement, which can cause significant dissatisfaction among users.
I believe that the direction of the relationship can be deterministically after the LLM generates a Cypher statement based on the provided schema.
Therefore, I am hosting this competition to help us find the best implementation of validating and fixing relationship directions in Cypher statements as accurately and fast as possible.

The idea is to use the winner's code and add it to LLM libraries like LangChain, LlamaIndex, and others.
By applying to this competition, you are allowing me, or others, to re-use the provided code in any commercial or non-commercial application with appropriate attribution.

## Rules of the competition

1. Python is preferred (although applications in other languages will be accepted)
2. No external libraries or tools shall be used. Only [standard, bundled libraries](https://en.wikipedia.org/wiki/Standard_library) apply.

The solutions will be judged based on the following three criterias, in the order given:

1. Accuracy of results
2. Readability of the code
3. Performance of the code

Prizes are the following:

1. TBD
2. TBD
3. TBD

Sponsored by Neo4j <3

## How to apply

TBD

## Test & Validation file

The `examples.csv` contains the dataset you can use to validate your implementation.
The CSV contains the following three columns:

- statement: Input Cypher statement that is used as the input to your implementation
- schema: Given graph schema that you can use to validate relationship direction and correct them if needed
- correct_query: Expected output 

The given schema is given as a list of triples where:
```
[(Person, WORKS_AT, Organization)]
```

- first element of the triple specifies the source or start node of a relationship
- second element of the triple specifies the relationship type
- third element of the triple specifies the target or end node of a relationship 

## Cypher direction validation guides

When I was preparing the Cypher examples, I followed these guidelines:

- If the given pattern doesn't fit the graph schema, simply return an empty string

- If the relationship is between two nodes of the same labels, there is nothing to validate or correct
```
(:Person)-->(:Person), (:Person)-[:KNOWS]->(:Person)
```
- If the input query has an undirected relationship in the pattern, we do not correct it.
```
(:Person)--(:Organization), (:Person)-[:WORKS_AT]-(:Organization)
```
- If a node label is missing in the defined pattern, we can still validate if it fits the graph schema
```
(:Person)-[:WORKS_AT]->()
```
- If the input query doesn't define the relationship type, but at least one node label is given of a pattern, we check if any relationship exists that matches the pattern and correct it if needed
```
(:Person)-->(), (:Organization)<-[r]-()
```

- When multiple relationships are given or a negation is used in a pattern, make sure that at least one of relationship types of the possible fits the given schema
```
(:Person)-[:KNOWS|WORKS_AT]->(:Organization), (:Person)-[:!KNOWS]->(:Organization)
```

- When variable length patten is used, we do not correct the direction
```
(:Person)-[:WORKS_AT*]->(:Person), (:Person)-[:WORKS_AT*1..4]->(:Person) 
```

## Disclaimer

All rights reserved to change the conditions of the competition if needed or see fit.

