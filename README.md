# Notes & handbook for vuestorefront with Elasticsearch
Error while yarn dev in vuestorefron-api <br/>
Change in /vue-storefront-api/tsconfig.json as per <a href="https://github.com/firebase/firebase-tools/issues/749#issuecomment-429598030">this</a> <br/>

<b>Creating New Mapping to existing index</b><br/>
<pre>PUT /vue_storefront_catalog/_mapping/vendor
{
  "properties": {
    "vendor": {
      "type": "keyword"
    }
  }
}</pre><br/>

<b>Inserting New data in Mapping.</b><br/>
<pre>http://localhost:9200/company/employee/?_create
POST
{
"name": "Andrew",
"age" : 45,
"experienceInYears" : 10
}</pre><br/>

<b>Getting Added Data <b><br/>
  <pre>http://localhost:9200/vue_storefront_catalog/vendor/_search</pre><br/>
