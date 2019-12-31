# Notes & handbook for vuestorefront with Elasticsearch
Error while yarn dev in vuestorefron-api <br/>
Change in /vue-storefront-api/tsconfig.json as per <a href="https://github.com/firebase/firebase-tools/issues/749#issuecomment-429598030">this</a> <br/>

<b>Creating New Mapping to existing index</b><br/>
<code>http://localhost:9200/company/employee/?_create
POST
{
"name": "Andrew",
"age" : 45,
"experienceInYears" : 10
}</code><br/>
