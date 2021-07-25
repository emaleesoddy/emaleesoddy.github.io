## apex enterprise patterns: system layers

### overview of system layers

#### service layer
* establishes a set of available operations and coordinates the application's response in each operation
* business tasks, calculations, and processes
* abstract vs. specific; other pieces of code "the client" invoke the service layer
* bulkification; should be able to handle lists of records
* security especially important at this layer, utilize "with sharing"
* throw exceptions; allow callers to interpret & handle these
* compound services vs. client needing to call multiple services; optimize SOQL & DML usage
* keep the service stateless, allow client to employ own state management

#### domain layer
* apex triggers; close to objects and record manipulation
* called directly and indirectly within service layer

#### selector layer

### other layers

#### utility layer

#### test layer

### resources
* [Apex Enterprise Patterns: Service Layer](https://trailhead.salesforce.com/content/learn/modules/apex_patterns_sl) on Trailhead
* [Apex Enterprise Patterns: Domain & Selector Layers](https://trailhead.salesforce.com/en/content/learn/modules/apex_patterns_dsl) on Trailhead