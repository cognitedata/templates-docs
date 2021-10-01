# Built-in schema
This is the current built-in schema

```graphql
scalar Long

type DatapointString implements Datapoint {
    timestamp: Long!
    value: Float
    stringValue: String
}

type DatapointFloat implements Datapoint {
    timestamp: Long!
    value: Float
}

type DatapointInt {
    timestamp: Long!
    value: Int
}

type MetadataItem {
    key: String
    value: String
}

type AggregationResult {
    average: [DatapointFloat]
    max: [DatapointFloat]
    min: [DatapointFloat]
    count: [DatapointInt]
    sum: [DatapointFloat]
    interpolation: [DatapointFloat]
    stepInterpolation: [DatapointFloat]
    continuousVariance: [DatapointFloat]
    discreteVariance: [DatapointFloat]
    totalVariation: [DatapointFloat]
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
    datapoints(start: Long, end: Long, limit: Int! = 100): [Datapoint]
    aggregatedDatapoints(start: Long, end: Long, limit: Int! = 100, granularity: String!): AggregationResult
    latestDatapoint: Datapoint
}

type SyntheticTimeSeries {
    name: String
    metadata: [MetadataItem]
    description: String
    isStep: Boolean
    unit: String
    datapointsWithGranularity(start: Long, end: Long, limit: Int! = 100, granularity: String): [Datapoint]
    datapoints(start: Long, end: Long, limit: Int! = 100): [Datapoint]
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

type File {
    id: Long!
    externalId: String
    name: String!
    directory: String
    mimeType: String
    source: String
    metadata: [MetadataItem]
    dataSetId: Long
    assets: [Asset]
    sourceCreatedTime: Long
    sourceModifiedTime: Long
    uploaded: Boolean!
    uploadedTime: Long
    createdTime: Long!
    lastUpdatedTime: Long!
    downloadUrl: String
}
```
