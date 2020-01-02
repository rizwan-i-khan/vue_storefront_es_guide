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
  
  
 <h2>Create new Entity/mapping for elasticsearch</h2><br/>
 <b>Step 1:</b><br/>
  <pre><b>Create New file in vuestorefront-api/src/graphql/elasticsearch/vendor/schema.graphqls</b>
    type Query {
      vendor(filter: VendorInput): ESResponse
    }
    input VendorInput
      @doc(description: "TaxRuleInput specifies the tax rules information to search") {
      entity_id: Int @doc(description: "An ID that uniquely identifies the vendor")
      is_seller: Int @doc(description: "Identifies if this is is seller or not.")
    }</pre>
    
<b>Step 2:</b><br/>
  <pre><b>Create New file in vuestorefront-api/src/graphql/elasticsearch/vendor/resolver.js</b>
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

After you have performed above steps verify your data on http://your_ip_addr:8080/graphiql
if everything goes well you can serahc your data by below query.

{
  my_custom_es_mapping {
    hits
  }
}
</pre>
    
<b> To use ES data you can create new api endpoint in vuestorefront-api to call elasticsearch for getting custom entity data.</b>
  <pre>
  api.get('/get_es_sellers', (req, res) => {
      let size = 10;
      let url = config.customapi.url + '/vue_storefront_catalog/vendor/_search';
      let qs = req.query;
      url = url+'?size=' + req.query.record_size;
      request(
            {url,json: true},
          (error, response, body) => {
              let apiResult;
              const errorResponse = error || body.error;
              if (errorResponse) {
                  apiResult = { code: 500, result: errorResponse };
              } else {
                  apiResult = { code: 200, result: body};
              }
              res.status(apiResult.code).json(apiResult);
            }
        );
    })
  </pre>

<b> I have directly called the above vuestorefront-api using axios method of vue, please create a new module or component to call to follow best practice :) </b>
<pre>
axios.get(my_new_api_url,{
    params: {
        record_size: '20',
        profileUrl: this.profile
    }
}).then(response => {
    console.log("results",response.data.result);
}).catch(e => {
    console.error(e)
})
</pre>

<b>Additional information</b>
<pre>
For checking schema of your new added mapping.
http://localhost:9200/vue_storefront_catalog/vendor/_mapping?pretty

For checking data of your new added mapping.
http://localhost:9200/vue_storefront_catalog/vendor/_search?pretty

In above links <b>vue_storefront_catalog</b> is our index name and <b>vendor</b> is our new entity/mapping name.
</pre>
