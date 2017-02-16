[![Build Status](https://travis-ci.org/zalando-incubator/spark-json-schema.svg?branch=scala_2.10)](https://travis-ci.org/zalando-incubator/spark-json-schema)
[![codecov.io](http://codecov.io/github/zalando-incubator/spark-json-schema/coverage.svg?branch=scala_2.10)](http://codecov.io/github/zalando-incubator/spark-json-schema?branch=master)
[![MIT licensed](https://img.shields.io/badge/license-MIT-green.svg)](https://raw.githubusercontent.com/zalando-incubator/spark-json-schema/spark_2.10/LICENSE)

# spark-json-schema

This goal of the spark-json-schema library is to support input data integrity when loading json data into Apache Spark.
For this purpose the library:
    
- Reads in an existing json-schema file
- Parses the json-schema and builds a Spark DataFrame schema

This generated schema can be used when loading json data into Spark.
This verifies that the input data conforms to the given schema and enables to filter out corrupt input data.
 

# Quickstart

Include the library under the following coordinates:
```scala
libraryDependencies += "org.zalando" %% "spark-json-schema" % "0.5"
```
Parse a given json-schema file by providing the path to the input file.
This file should be relative to the resources folder:
```scala
val schema = SchemaConverter.convert("schemaFile.json")
```
Use the created schema when loading json-files into Spark:
```scala
val dataFrame = sqlContext.read.schema(schema).json(source)
```
Jsons that are not according to the schema have only `null` in the dataFrame fields and can be filtered out:
```scala
val filteredRdd = inputRdds.filter { row =>
      val notValid = Range(0, row.length).map(index => row.get(index)).contains(null)
      if (notValid) {
        // TODO: Do something with dropped JSONs
      }
      !notValid
    }
```
After these steps you can be sure that all files that were loaded for further processing comply to your schema.

# Contact

Feel free to contact us at team-payana@zalando.de

# License

The MIT License (MIT) Copyright © 2016 Zalando SE, https://tech.zalando.com

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
