## Firebase Config 넣기

``` 
ㄴㅇㄹㄴㅇㄹ
```


```markdown
<pre><code>
////////////////////////////////////-----  dbPost  -----//////////////////////////////////////////////////////
exports.write_post = functions.firestore.document('post/{post_key}').onWrite(async (snap, context) => {
  console.log("======================== write_post : ", "okok : ");
  const postKey = context.params.post_key;
  console.log("postKey : ", postKey);
  const oldData = snap.before.data();
  console.log("oldData : ", oldData);
  const newData = snap.after.data();
  console.log("newData : ", newData);
// const index_type =  newData ? 'post/' + newData.type + '/' : 'post/' + oldData.type + '/';  
  // const result = index_type.toLowerCase();
  // console.log("index_type : ", result);
  // let elasticsearchFields = ['model', 'manufacturer', 'description', 'transmission_type', 'fuel_type', 'noise_level',
  //   'euro_standard', 'year', 'co2', 'noise_level', 'urban_metric', 'extra_urban_metric', 'combined_metric'];
  let elasticSearchConfig = functions.config().elasticsearch;
  let elasticSearchMethod = newData ? 'POST' : 'DELETE';
  // let elasticSearchUrl = elasticSearchConfig.url + result + post_key;
  // let elasticSearchUrl = elasticSearchConfig.url + 'post/' + post_key;
  let elasticSearchUrl = elasticSearchConfig.url + 'post/_doc/' + postKey;
  console.log("elasticSearchUrl : ", elasticSearchUrl);
  let elasticsearchRequest = {
    method: elasticSearchMethod,
    url: elasticSearchUrl,
    auth: {
      username: elasticSearchConfig.username,
      password: elasticSearchConfig.password,
    },
    body: newData,
    json: true
  };
  return request(elasticsearchRequest).then(response => {
    console.log('write_post Elasticsearch response', response);
  })
});
</pre></code>
```






You can use the [editor on GitHub](https://github.com/KennKim/Firebase/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/KennKim/Firebase/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
