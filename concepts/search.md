# Search (Very experimental)
This is a proof of concept of searchable fields in a template.
Prerequisite for using this is that the user has write access to assets. Note that the service will then create dummy assets on behalf of the user, which might create noise for the current project, so this feature should **ONLY** be used in personal / experimental projects.

## How to use
Up to two fields can be searchable, by adding a `@search` directive. Also the field has to be of type string to be searchable.
```graphql
type Well @template {
    name: String @search
}
```

Only constant resolvers are supported as resolvers for searchable fields. So this is a valid template instance for searchable fields.
```python
TemplateInstance(
    external_id="well1",
    template_name="Well",
    field_resolvers={
        "name": ConstantResolver(value="MyWell"),
    },
),
```

The `@search` directive generates an endpoint in the root query object, which is called `{templateName}Search({searchFieldName1}: String, {searchFieldName2}: String): [{templateName}]`. So for the example above, we now have a `wellSearch(name: String): [Well]` in the top level query object.

Example query:
```graphql
{
    wellSearch(name: "MyWell") {
      name
    }
}
```

**Note** that the search index is eventual consistent, so it might take some time from you ingest an instance for it to be available in the search results.

## Current Implementation
The current implementation creates a root asset node with externalId `__domains`, and a child asset per domain and version, with the externalId `__domains_{domainName}_{version}`. Each indexed template instance has it's own asset as a leaf in this structure, with externalId `__domains_{domainName}_{version}_{externalId}`. 
