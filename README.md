# LinkedIn-API数据调研

## API使用权限
### 申请权限流程
1. 开发者网站注册 

   [https://developer.linkedin.com](url)

2. 创建app
- 需要备注产品的名字
- 在领英注册公司的链接（必填）
- 隐私政策链接
- logo图片

3. 获取api-key
- client_id
- client_secret

1. api使用权限条款

   [https://www.linkedin.com/legal/l/api-terms-of-use](url)

- 注意：可以设置url的重定向

### 申请令牌
- HTTPS请求

<pre><code>POST https://www.linkedin.com/oauth/v2/accessToken HTTP/1.1

Content-Type: application/x-www-form-urlencoded
grant_type=client_credentials
client_id={your_client_id}
client_secret={your_client_secret}</code></pre>

- 返回参数
<pre><code>{
    "access_token": "AQV8...",
    "expires_in": "1800"
}</code></pre>


## API功能
### 查询限制
1. 查询链接

<pre><code>host: api.linkedin.com
basePath: /v2
scheme: https</code></pre>

2. 数据输入/输出格式
<pre><code>Content-Type: application/json</code></pre>

3. 错误码返回

   [https://learn.microsoft.com/en-us/linkedin/shared/api-guide/concepts/error-handling?context=linkedin%2Fcontext
](url)
4. 分页

<pre><code>GET https://api.linkedin.com/v2/{service}?start=10&count=10</code></pre>


5. 访问限制
按照申请的key来决定

6. 方法请求

   [https://learn.microsoft.com/en-us/linkedin/shared/api-guide/concepts/methods?context=linkedin%2Fcontext](url)

### 工作操作
1. 申请/删除/更新工作

   [https://learn.microsoft.com/en-us/linkedin/talent/job-postings/api/sync-job-postings](url)

1. 创建工作
- 链接

  [https://learn.microsoft.com/en-us/linkedin/talent/apply-connect/sync-jobs-onsite-apply
](url)
- HTTPS接口
<pre><code>POST https://api.linkedin.com/v2/simpleJobPostings
Connection: Keep-Alive
Authorization: Bearer {access_token}
x-restli-method: batch_create</code></pre>

- 请求主体
<pre><code>{
 "elements": [{
   "integrationContext": "urn:li:organization:2414183",
   "companyApplyUrl": "http://linkedin.com",
   "description": "工作介绍",
   "employmentStatus": "PART_TIME",
   "externalJobPostingId": "1234",
   "listedAt": 1440716666,
   "jobPostingOperationType": "CREATE",
   "title": "Software Engineer",
   "location": "India",
   "workplaceTypes": [
    "remote"
   ]
  }]
}</code></pre>

- 主体参数描述
   
   [https://learn.microsoft.com/en-us/linkedin/talent/job-postings/api/job-posting-api-schema](url)

- 注意：

   上述请求主体的第一个元素是在LinkedIn上发布远程工作的示例（工作场所类型是remote的，请注意，location字段只有COUNTRY值），第二个元素是发布混合工作的示例（工作场所类型是hybrid的）。在第三个元素中，由于没有为workplaceTypes字段提供价值，该工作将作为On-site工作发布。


### 搜索接口
- 接口文档：


  [https://developer.linkedin.com/docs/guide/v2](url)

  查询的是job和people接口，当前接口受到限制，必须需要走商务的人才计划，按照接入ast才能拿到数据。

- 填写表格

  [https://business.linkedin.com/talent-solutions/ats-partners/partner-application](url)

- 将ast插件集成到前端
  
  [https://learn.microsoft.com/en-us/linkedin/talent/apply-connect/customer-configuration](url)

- HTTPS请求

<pre><code>POST https://api.linkedin.com/v2/atsIntegrations?ids[0].integrationContext={integrationContext}&ids[0].integrationType={integrationType}&ids[0].tenantType={tenantType}&ids[0].dataProvider=ATS
Authorization: Bearer {token}
x-restli-method: batch_partial_update
</code></pre>

- 请求主体

<pre><code>{
  "entities": {
    "integrationContext=urn:li:organization:1234&integrationType=APPLY_CONNECT&tenantType=JOBS&dataProvider=ATS": {
      "patch": {
        "$set": {
          "integrationName": "My Customer's LinkedIn Apply Connect Integration"
        }
      }
    }
  }
}
</code></pre>


- 注意
目前这里都是黑盒，这里需要看到ast以后在仔细研究一下数据来源，目前能够给到的文档都是由ast这个集成库获取数据，这个借口参数如何，需要商务对接才能看到。

### IM接口
- 链接

  [https://learn.microsoft.com/en-us/linkedin/shared/integrations/communications/messages?context=linkedin%2Fcompliance%2Fcontext&view=li-lms-unversioned&preserve-view=true](url)

- HTTPS接口
<pre><code>POST https://api.linkedin.com/v2/messages
Connection: Keep-Alive
Authorization: Bearer {access_token}
x-restli-method: batch_create</code></pre>

- 请求主体
<pre><code>{
  "recipients": [
    "urn:li:person:123ABC",
    "urn:li:person:456DEF"
  ],
  "subject": "Group conversation title",
  "body": "Hello everyone! This is a message conversation to demo the Message API.",
  "messageType": "MEMBER_TO_MEMBER",
  "attachments": [
    "urn:li:digitalMediaAsset:123ABC"
  ]
}</code></pre>


