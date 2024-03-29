![](https://img.shields.io/pypi/v/foliantcontrib.project_graph.svg)

# ProjectGraph

Foliant Meta Command which draws a scheme of project sections. This extension uses meta-information, collected by folinatcontrib.meta extension.

[Graphviz](http://graphviz.org) is used to build a scheme.

`libgraphviz-dev` is required to be installed on your machine.

## Installation

```bash
$ pip install foliantcontrib.project_graph
```

## Usage

First you need to specify all relations between the documents in your project. To do this add the `relates` section to your document's meta-data:

```yaml
---
relates:
    - tests/test1.md
    - specs/spec.md
---
```

in `relates` section you need to specify a list of documents to which current document relates. You can specify either a relative path to connected document or its ID (if the corresponding document has an ID assigned in its meta section):


```yaml
# index.md
---
id: index
---
```

```yaml
# glossary.md
---
relates:
    - index
---
```

After you specified all relations, run the draw command:

```bash
$ foliant draw
```

Scheme will appear in the file `project_graph.png`

## Config

ProjectGraph has a number of options:

```yaml
project_graph:
    directed: false
    filename: project_graph.png
    gv_attributes:
        node:
            shape: rect
            color: green
        edge:
            arrowhead: open
        graph:
            ranksep: 1
        main_relation:
            penwidth: 2
```

`directed`
:   Specifies graph to be directed or not. Default: `false`

`filename`
:   Graph output filename. Default: `project_graph.png`

`gv_attributes`
:   A dictionary with global attributes of the graph. Each dictionary should be stored under the Graphviz Entity key (`node`, `edge`, or `graph`), or under type key. All sections or relations which have this type will get these attributes.

If you want to adjust the look of just one node, add a `gv_attributes` option into the meta of the document:

```yaml
---
id: index
relates:
    - glossary
gv_attributes:
    color: green
    shape: circle
---
```

You can also change the look of the edges, which connect nodes. To do this you can use a detailed syntax of relations.

## Relations detailed syntax

As stated in the beginning, to specify relations you need to add a `relates` param and include a list of related documents IDs\\file paths:

```yaml
---
relates:
    - doc1.md
    - MAIN_SPEC
---
```

But there's also a detailed syntax for specifying relations, it looks like this:

```yaml
---
relates:
    - rel_path: doc1.md
      type: details
    - rel_id: MAIN_SPEC
      gv_attributes:
        color: #CCCCCC
        arrowhead: none
---
```

In the detailed syntax each relation is not a string, but a mapping. This time you have to explicitly use either `rel_path` key, if you are pointing to a document by path, or `rel_id` if you do it by ID.

Also you can specify relation type by adding a `type` key. Right now the value of this key just goes to the edge label, but soon you'll be able to change the appearance of all edges with one type.

Finally you can override this specific edge's appearance by adjusting Graphviz attributes in the `gv_attributes` key.
