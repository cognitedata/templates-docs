# Built-in schema
This is the current built-in schema

```graphql
scalar Long

type Datapoint {
    timestamp: Long!
    value: Float
}

type DatapointInt {
    timestamp: Long!
    value: Int
}

type Datapoints {
    id: Long
    externalId: String
    isString: Boolean
    isStep: Boolean
    unit: String
    datapoints: [Datapoint]
}

type MetadataItem {
    key: String
    value: String
}

type AggregationResult {
    average: [Datapoint]
    max: [Datapoint]
    min: [Datapoint]
    count: [DatapointInt]
    sum: [Datapoint]
    interpolation: [Datapoint]
    stepInterpolation: [Datapoint]
    continuousVariance: [Datapoint]
    discreteVariance: [Datapoint]
    totalVariation: [Datapoint]
}

type TimeSeries {
    id: Long
    name: String
    metadata: [MetadataItem]
    description: String
    externalId: String
    isString: Boolean
    isStep: Boolean
    unit: String
    datapoints(start: Long, end: Long, limit: Int): [Datapoint]
    aggregatedDatapoints(start: Long, end: Long, limit: Int, granularity: String!): AggregationResult
}

type Asset {
    id: Long!
    externalId: String
    name: String!
    description: String
    root: Asset
    parent: Asset
    source: String
    metadata: [MetadataItem]
}
```
