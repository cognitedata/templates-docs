# Template Instance
Template instances are implementing a specific template type by specifying what happens when a template field is queried. We call this concept of resolving a field for a field resolver. So each field of an instance is bound to a field resolver. And there are multiple types of field resolvers. Each template instance also has an externalId that identifies the instance.

## Field Resolvers
This is a list of supported field resolvers today.

### Constant Resolver
The constant resolver, resolves a field to a value that is only persisted in the domain itself. The rationale for calling it constant, is that it can't change unless the instance itself change. This is in contrast to other resolvers, which dynamically resolves the value.

A constant resolver can resolve to any JSON type, eg. string, number, array or object.

Example in Python
```python
ConstantResolver(value="Foo")
ConstantResolver(value=["Foo", "Bar"])
ConstantResolver(value={ "test": 0.2, "bar": 0.5 })
```

### Time Series Resolver
The Time Series resolver, resolves a field of type `TimeSeries` (a [built-in type](./built-in-schema.md)) to a time series in CDF. All you have to say is the externalId of the time series.

Example in Python:
```python
TimeSeriesResolver("some_ts_ext_id")
```

### Synthetic Time Series Resolver
The Synthetic Time Series resolver, resolves a field of type `TimeSeries` (a [built-in type](./built-in-schema.md)) to a synthetic time series in CDF. The only required argument is the synthetic time series expression, however it also supports setting the description, the unit, if it's a step series or if it's a string series.

Example in Python:
```python
SyntheticTimeSeriesResolver(
    expression="sin(pow(TS{externalId='Norway_confirmed'}, 2))",
    description="Weird sin time series",
    is_step=False,
    is_string=False,
    unit="radians",
)
```

### RAW Resolver
The RAW resolver, resolves a field to a value coming from a specific row and column in RAW.

Example in Python:
```python
RawResolver(
    db_name="SomeDb",
    table_name="SomeTable",
    row_key="someRow",
    column_name="someColumn"
)
```

## Relationships
Built-in types and templates can relate to each other.
For example if we have the following schema:
```graphql
type System @template {
    wells: [Well]
}
type Well @template {
    asset: Asset,
    assets: [Asset]
}
```

You can encode the relationship between templates or built-in types by specifying the externalId of the relating instances.
The externalIds can originate from any field resolver which resolves to a string or an array of strings, depending if it's a one or one to many relation.

```python
[
    TemplateInstance(
        external_id="someSystem",
        template_name="System",
        field_resolvers={
            "wells": ConstantResolver(value=["well1"]),
        },
    ),
    TemplateInstance(
        external_id="well1",
        template_name="Well",
        field_resolvers={
            "asset": ConstantResolver(value="someAssetId"),
            "assets": ConstantResolver(value=["someAssetExtId", "someOtherAssetExtId"]),
        },
    )
]
```
