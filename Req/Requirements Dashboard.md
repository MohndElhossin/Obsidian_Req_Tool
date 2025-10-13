```dataview
TABLE  Requirement_ID ,Feature_Assignment, Requirement_Type, Priority, Status, Upstream, Downstream
FROM ""
WHERE contains(file.name, "Featurex") OR contains(file.name, "featurex")
```