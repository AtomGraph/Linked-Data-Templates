# Linked Data Templates

Linked Data Templates (LDT) is a uniform protocol for read-write Linked Data. This document defines an abstract syntax (a data model) of Linked Data applications with SPARQL backends, and semantics of CRUD interactions against their resources.

LDT can be used to design ontology-driven Web application programming interfaces (APIs). It provides facilities to define interactions with application resources declaratively using SPARQL commands. It also provides a standard method to evaluate requests into responses over application ontology and dataset.

LDT specification draft is published by the [Declarative Linked Data Apps Community Group](https://www.w3.org/community/declarative-apps/) and hosted on https://github.com/AtomGraph/Linked-Data-Templates/

## Architecture

![LDT architecture](https://atomgraph.github.io/Linked-Data-Templates/images/components.svg "LDT architecture")

## Example

This is an example of an LDT ontology with templates and queries:
```turtle
@prefix :     <https://linkeddatahub.com/ns#> .
@prefix ldt:  <https://www.w3.org/ns/ldt#> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix sp:   <http://spinrdf.org/sp#> .
@prefix spl:  <http://spinrdf.org/spl#> .

: a ldt:Ontology ;
  owl:imports ldt:, sp: ;
  rdfs:label "Example ontology" .

:PersonItem a ldt:Template ;
  rdfs:label "Person item" ;
  ldt:match "/people/{familyName}" ;
  ldt:param :GraphParam ;
  ldt:query :DescribeWithTopic ;
  ldt:update :DeleteWithTopic ;
  rdfs:isDefinedBy : .

:GraphParam a ldt:Parameter ;
  rdfs:label "Graph parameter" ;
  spl:predicate :g ;
  spl:valueType rdfs:Resource ;
  spl:optional true ;
  rdfs:isDefinedBy : .

:DescribeWithTopic a ldt:Query, sp:Describe ;
  rdfs:label "Describe with topic" ;
  sp:text """PREFIX  foaf: <http://xmlns.com/foaf/0.1/>

DESCRIBE ?this ?primaryTopic
WHERE
  { GRAPH ?g
      { ?this  ?p  ?o
        OPTIONAL
          { ?this     foaf:primaryTopic  ?primaryTopic .
            ?primaryTopic
                      ?primaryTopicP     ?primaryTopicO
          }
      }
  }""" ;
  rdfs:isDefinedBy : .

:DeleteWithTopic a ldt:Update, sp:Modify ;
  rdfs:label "Delete with topic" ;
  sp:text """PREFIX  foaf: <http://xmlns.com/foaf/0.1/>

DELETE {
  GRAPH ?g {
    ?this ?p ?o .
    ?primaryTopic ?primaryTopicP ?primaryTopicO .
  }
}
WHERE
  { GRAPH ?g
      { ?this ?p ?o
        OPTIONAL
          { ?this foaf:primaryTopic ?primaryTopic .
            ?primaryTopic ?primaryTopicP ?primaryTopicO
          }
      }
  }""" ;
  rdfs:isDefinedBy : .
  ```

## Implementations

[AtomGraph Processor](https://github.com/AtomGraph/Processor) is an LDT processor and server. It includes an LDT test suite.

## Contributions

The specification draft is published from the [index.html](https://github.com/AtomGraph/Linked-Data-Templates/blob/gh-pages/index.html) document in the `gh-pages` branch.
Please raise issues or create pull requests.
