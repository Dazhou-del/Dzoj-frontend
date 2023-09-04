# Dzoj-frontend

```
openapi --input http://localhost:8121/api/v2/api-docs --output ./generated --client axios
```

## Project setup

```
yarn install
```

### Compiles and hot-reloads for development

```
yarn serve
```

### Compiles and minifies for production

```
yarn build
```

### Lints and fixes files

```
yarn lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

### OpenAPI object
    The library exposes a global OpenAPI object that can be used to configure the requests, below you can find the properties and their usage.
    Example:
```
    export const OpenAPI: OpenAPIConfig = {
    BASE: 'http://localhost:3000/api',
    VERSION: '2.0',
    WITH_CREDENTIALS: false,
    CREDENTIALS: 'include',
    TOKEN: undefined,
    USERNAME: undefined,
    PASSWORD: undefined,
    HEADERS: undefined,
    ENCODE_PATH: undefined,
};
```
### Properties
#### OpenAPI.BASE
The base path of the OpenAPI server, this is generated from the spec, but can be overwritten to switch servers.
```
if (process.env === 'development') {
    OpenAPI.BASE = 'http://staging.company.com:3000/api';
}
if (process.env === 'production') {
    OpenAPI.BASE = '/api';
}
```
#### OpenAPI.VERSION
The version param in the OpenAPI paths {api-version}. The version is taken from the spec, but can be updated to call multiple versions of the same OpenAPI backend.
OpenAPI 路径{ api-version }中的版本参数。该版本取自规范，但可以进行更新，以调用同一 OpenAPI 后端的多个版本。
#### OpenAPI.WITH_CREDENTIALS
Similar to the withCredentials property of the XHR specification. When set to true, cross-site requests should be made using credentials such as cookies, authorization headers, etc.
类似于 XHR 规范的 withCredenals 属性。当设置为 true 时，跨站点请求应使用诸如 cookie、授权头等凭据进行。
#### OpenAPI.CREDENTIALS
Similar to the credentials property of the Fetch specification. When OpenAPI.WITH_CREDENTIALS is set to true, this property controls the specific implementation for Fetch and Node-Fetch clients. Valid values are: include, omit and same-origin.
类似于 Fetch 规范的凭据属性。当 OpenAPI.WITHCREDENTIALS 设置为 true 时，此属性控制 Fetch 和 Node-Fetch 客户端的特定实现。有效值是: include、 omit 和 same-source。
#### OpenAPI.TOKEN
Set the Bearer authentication token to use for the requests. This needs to be a valid (non-expired) token, otherwise the request will fail. The property can be updated as often as you want, this is useful for scenario's where the token would automatically refresh after x minutes. This property also allows you to use an async method that will be resolved before requests are made.
设置用于请求的 Bearer 身份验证标记。这需要是一个有效的(非过期的)令牌，否则请求将失败。属性可以随时更新，这对于令牌在 x 分钟后自动刷新的场景非常有用。此属性还允许您使用将在发出请求之前解析的异步方法。
```
OpenAPI.TOKEN = 'MY_TOKEN';

OpenAPI.TOKEN = async () => {
    // Note: loading this from a JSON file is not recommended ;-)
    const response = await fetch('configuration.json');
    const { token } = response.json();
    return token;
};
```
#### OpenAPI.USERNAME
Set the basic authentication username, although not recommended, the basic authentication header is still supported. The username and password hash will be calculated by the client before sending the request. This property also allows you to use an async method that will be resolved before requests are made.
设置基本身份验证用户名，虽然不推荐，但仍然支持基本身份验证标头。客户端将在发送请求之前计算用户名和密码散列。此属性还允许您使用将在发出请求之前解析的异步方法。
```
OpenAPI.USERNAME = 'john';

OpenAPI.USERNAME = async () => {
    // Note: loading this from a JSON file is not recommended ;-)
    const response = await fetch('configuration.json');
    const { username } = response.json();
    return username;
};
```
#### OpenAPI.PASSWORD
Set the basic authentication password. See OpenAPI.USERNAME for more info.
设置基本的身份验证密码。更多信息请参见 OpenAPI.USERNAME。
```
OpenAPI.PASSWORD = 'welcome123';

OpenAPI.PASSWORD = async () => {
    // Note: loading this from a JSON file is not recommended ;-)
    const response = await fetch('configuration.json');
    const { password } = response.json();
    return password;
};
```
#### OpenAPI.HEADERS
This property allows you to specify additional headers to send for each request. This can be useful for adding headers that are not generated through the spec. Or adding headers for tracking purposes. This property also allows you to use an async method that will be resolved before requests are made.
此属性允许您为每个请求指定要发送的附加标头。这对于添加不是通过规范生成的标头非常有用。或者为跟踪目的添加标头。此属性还允许您使用将在发出请求之前解析的异步方法。
```
OpenAPI.HEADERS = {
    'x-navigator': window.navigator.appVersion,
    'x-environment': process.env,
    'last-modified': 'Wed, 21 Oct 2015 07:28:00 GMT',
};

OpenAPI.HEADERS = async () => {
    // Note: loading this from a JSON file is not recommended ;-)
    const response = await fetch('configuration.json');
    const { headers } = response.json();
    return headers;
};
```
#### OpenAPI.ENCODE_PATH
By default, all path parameters are encoded using the encodeURI method. This will convert invalid URL characters, for example spaces, backslashes, etc. However, you might want to make the encoding more strict due to security restrictions. So you can set this to encodeURIComponent to encode most non-alphanumerical characters to percentage encoding. Or set a customer encoder that just replaces some special characters.
默认情况下，所有路径参数都使用 encodeURI 方法进行编码。这将转换无效的 URL 字符，例如空格、反斜杠等。但是，由于安全限制，您可能希望使编码更加严格。因此可以将其设置为 encodeURIComponent，将大多数非字母数字字符编码为百分比编码。或者设置一个客户编码器，只替换一些特殊字符。
```
OpenAPI.ENCODE_PATH = encodeURIComponent;

OpenAPI.ENCODE_PATH = (value: string) => {
    return value.replace(':', '_');
};
```
