# Notes & handbook for vuestorefront with Elasticsearch
Error while yarn dev in vuestorefron-api <br/>
Change in /vue-storefront-api/tsconfig.json as per <a href="https://github.com/firebase/firebase-tools/issues/749#issuecomment-429598030">this</a> <br/>

<b>Creating New Mapping to existing index</b><br/>
<pre>PUT /vue_storefront_catalog/_mapping/vendor
{
  "properties": {
    "v_id": {
      "type": "long"
    }
  }
}</pre><br/>

<b>Inserting New data in Mapping.</b><br/>
<pre>POST /vue_storefront_catalog/vendor/1/
{
"name": "Rizwan Khan",
"age" : 25,
"experienceInYears" : 3
}</pre><br/>

<b>Getting Added Data </b><br/>
  <pre>http://localhost:9200/vue_storefront_catalog/vendor/_search</pre><br/>
  
 <b>Getting mapping of particular index of ES </b><br/>
  <pre>http://localhost:9200/vue_storefront_catalog/vendor/_mapping?pretty</pre><br/>

<b>Deleting data from particular mapping type, we can not delete whole mapping as per ES guide. execute below with KIBANA </b><br/>
  <pre>DELETE /index_name/mapping_type_name/id/</pre><br/>

 <b>Delete all index and all mapping + data in ES </b><br/>
  <pre>curl -X DELETE 'http://localhost:9200/_all'</pre><br/>
  
  
 <b>Create new Entity/mapping for elasticsearch</b><br/>
 <b>Step 1:</b><br/>
  <pre>
      <b>Create New file in vuestorefront-api/src/graphql/elasticsearch/vendor/schema.graphqls</b>
      type Query {
        vendor(filter: VendorInput): ESResponse
      }
      input VendorInput
        @doc(description: "TaxRuleInput specifies the tax rules information to search") {
        entity_id: Int @doc(description: "An ID that uniquely identifies the vendor")
        is_seller: Int @doc(description: "Identifies if this is is seller or not.")
      }
  </pre>
<b>Step 2:</b><br/>
  <pre>
      <b>Create New file in vuestorefront-api/src/graphql/elasticsearch/vendor/resolver.js</b>
      import config from 'config';
      import client from '../client';
      import { buildQuery } from '../queryBuilder';

      async function vendor(filter) {
        let query = buildQuery({ filter, pageSize: 150, type: 'vendor' });
        const response = await client.search({
          index: config.elasticsearch.indices[0],
          type: config.elasticsearch.indexTypes[6],
          body: query,
        });
        return response;
      }
      const resolver = {
        Query: {
          vendor: (_, { filter }) => vendor(filter),
        },
      };
      export default resolver;
  </pre>
