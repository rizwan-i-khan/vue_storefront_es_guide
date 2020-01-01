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
