---
layout: page
title: BuildUp Part Library Syntax
permalink: /syntax/builduplibrary
---

[YAML]: https://en.wikipedia.org/wiki/YAML

Part libraries can be used to generate identically formatted markdown files for a number of components. The components are defined in a [YAML] file. The top level key specifies the part.


For example, `Screws.yaml` contains two parts: `M3x6Cap_SS` and `M3x8Cap_SS`

```
M3x6Cap_SS:
    Name: M3 x 6 mm Cap Screw (Stainless Steel)
    Description: Metric Screw
    Specs:
        Material: A2 (18-8) Stainless Steel
    Suppliers:
        McMasterCarr:
            PartNo: 91292A111
            Link: 'https://www.mcmaster.com/91292A111'
        RS Components:
            PartNo: 280-981
            Link: 'https://uk.rs-online.com/web/p/socket-screws/0280981/'
M3x8Cap_SS:
    Name: M3 x 8 mm Cap Screw (Stainless Steel)
    Description: Metric Screw
    Specs:
        Material: A2 (18-8) Stainless Steel
    Suppliers:
        McMasterCarr:
            PartNo: 91292A112
            Link: 'https://www.mcmaster.com/91292A112'
        RS Components:
            PartNo: 280-997
            Link: 'https://uk.rs-online.com/web/p/socket-screws/0280997/'
```

To reference these screws could be used as parts using the following:

    You will need both [M3x6](Screws.yaml#M3x6Cap_SS){Qty: 10} and an [M3x8](Screws.yaml#M3x8Cap_SS){Qty: 10} screws
    
# Defined keys

The following keys are defined for a part in a part library:

* `Name` - The name of the part. Value should be text.
* `Description` - A description of the part. Value should be text or markdown.
* `Specs` - The specifications of the part. Value is key value pairs. All keys and values will be displayed in the result.
* `Suppliers` - The suppliers of the part. Value is a key for each supplier. The value for this key is two key value pairs with keys:
  * `PartNo` - Specifying the part number
  * `Link` - Specifying the link to the part

Extra information in the YAML file is allowed but is ignore. More defined keys are needed. Raise an issue if you have suggestions.
