# Web Scraper Github

Use the "Run" button to execute the code.


```python

```

## Project Outline:
- Website to Scrape - https://github.com/topics
- We will get topic title/page URL/description
- Each topic extracting the top 30 repositories 
- Obtain the Name,Username,Stars and Repo URL for each Repository

### Installing Requests 
- Requests makes handling HTTP requests simple 
- Requests is Open-Source HTTP Library. 


```python
!pip install requests --upgrade 
import requests
```

    Requirement already satisfied: requests in /opt/conda/lib/python3.9/site-packages (2.26.0)
    Collecting requests
      Downloading requests-2.28.1-py3-none-any.whl (62 kB)
    [K     |████████████████████████████████| 62 kB 1.5 MB/s eta 0:00:011
    [?25hRequirement already satisfied: certifi>=2017.4.17 in /opt/conda/lib/python3.9/site-packages (from requests) (2021.5.30)
    Requirement already satisfied: idna<4,>=2.5 in /opt/conda/lib/python3.9/site-packages (from requests) (3.1)
    Requirement already satisfied: charset-normalizer<3,>=2 in /opt/conda/lib/python3.9/site-packages (from requests) (2.0.0)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in /opt/conda/lib/python3.9/site-packages (from requests) (1.26.7)
    Installing collected packages: requests
      Attempting uninstall: requests
        Found existing installation: requests 2.26.0
        Uninstalling requests-2.26.0:
          Successfully uninstalled requests-2.26.0
    Successfully installed requests-2.28.1


### Choosing a Website
The Website to be Scraped is: https://github.com/topics


```python
topics_url = "https://github.com/topics"
```

#### HTTP Response Status Code 
- Informational responses (100 – 199)
- Successful responses (200 – 299)
- Redirection messages (300 – 399)
- Client error responses (400 – 499)
- Server error responses (500 – 599)


```python
response =  requests.get(topics_url)
response.status_code 
```




    200




```python
# This line extracts the number of charecters on the selected web page
len(response.text)
```




    152462



When we use Python to make a request to a certain URI, a response object is returned. Now, this response object would be used to access certain aspects like content, headers and more. 

 `response.text` returns the content of the response.


```python
page_contents = response.text 
page_contents[:1000]
```




    '\n\n<!DOCTYPE html>\n<html lang="en" data-color-mode="auto" data-light-theme="light" data-dark-theme="dark" data-a11y-animated-images="system">\n  <head>\n    <meta charset="utf-8">\n  <link rel="dns-prefetch" href="https://github.githubassets.com">\n  <link rel="dns-prefetch" href="https://avatars.githubusercontent.com">\n  <link rel="dns-prefetch" href="https://github-cloud.s3.amazonaws.com">\n  <link rel="dns-prefetch" href="https://user-images.githubusercontent.com/">\n  <link rel="preconnect" href="https://github.githubassets.com" crossorigin>\n  <link rel="preconnect" href="https://avatars.githubusercontent.com">\n\n\n\n  <link crossorigin="anonymous" media="all" rel="stylesheet" href="https://github.githubassets.com/assets/light-719f1193e0c0.css" /><link crossorigin="anonymous" media="all" rel="stylesheet" href="https://github.githubassets.com/assets/dark-0c343b529849.css" /><link data-color-theme="dark_dimmed" crossorigin="anonymous" media="all" rel="stylesheet" data-href="https://github.gith'




```python
#response.text
with open('webpage.html', 'w') as f:
    f.write(page_contents)
```

## Installing Beautiful Soup 

Python package called Beautiful Soup is used to parse HTML and XML texts. For parsed pages, it generates a parse tree that can be used to extract HTML data for web scraping.


```python
# installing beautiful soup 
!pip install beautifulsoup4 --upgrade 
```

    Requirement already satisfied: beautifulsoup4 in /opt/conda/lib/python3.9/site-packages (4.10.0)
    Collecting beautifulsoup4
      Downloading beautifulsoup4-4.11.1-py3-none-any.whl (128 kB)
    [K     |████████████████████████████████| 128 kB 7.6 MB/s eta 0:00:01
    [?25hRequirement already satisfied: soupsieve>1.2 in /opt/conda/lib/python3.9/site-packages (from beautifulsoup4) (2.0.1)
    Installing collected packages: beautifulsoup4
      Attempting uninstall: beautifulsoup4
        Found existing installation: beautifulsoup4 4.10.0
        Uninstalling beautifulsoup4-4.10.0:
          Successfully uninstalled beautifulsoup4-4.10.0
    Successfully installed beautifulsoup4-4.11.1



```python
# From the bs4 module you import the BeautifulSoup Class 
from bs4 import BeautifulSoup
```


```python
# To parse HTML Code, go to the documentation 
doc = BeautifulSoup(page_contents, 'html.parser')

# It is a BeautifulSoup Object 
type(doc) 
```




    bs4.BeautifulSoup



## Obtaining the Topic Titles, Topic Descriptions and Topic URLs
For the webpage at https://github.com/topics 


```python
# To obtain p tags in general, but it may obtain p_tags (headings/titles) that we don't require
p_tag = doc.find_all('p')
len(p_tag)
```




    67




```python
# To get the first 3 p-tags 
p_tag[:3]
```




    [<p class="f4 color-fg-muted col-md-6 mx-auto">Browse popular topics on GitHub.</p>,
     <p class="f3 lh-condensed text-center Link--primary mb-0 mt-1">
             Bootstrap
           </p>,
     <p class="f5 color-fg-muted text-center mb-0 mt-1">Bootstrap is an HTML, CSS, and JavaScript framework.</p>]



![image-3.png](attachment:image-3.png)
This allows us to reference the Title Name

### Extracting the Topic Titles 
`selection_class` contains the a reference to the relevant class in order to obtain the Title.
`topic_title_tags` finds all the paragraphs with respect to the selection_class.
`topic_titles` is a list that contains each topic's title


```python
# <p class="f3 lh-condensed mb-0 mt-1 Link--primary">3D</p>

# In order to get headings in order
# To be more specific with the p_tags that are extracted from the web page 
selection_class = "f3 lh-condensed mb-0 mt-1 Link--primary"
topic_title_tags = doc.find_all('p', {'class': selection_class})
len(topic_title_tags)
```




    30




```python
topic_titles = [] 
for tag in topic_title_tags:
    topic_titles.append(tag.text)
topic_titles
```




    ['3D',
     'Ajax',
     'Algorithm',
     'Amp',
     'Android',
     'Angular',
     'Ansible',
     'API',
     'Arduino',
     'ASP.NET',
     'Atom',
     'Awesome Lists',
     'Amazon Web Services',
     'Azure',
     'Babel',
     'Bash',
     'Bitcoin',
     'Bootstrap',
     'Bot',
     'C',
     'Chrome',
     'Chrome extension',
     'Command line interface',
     'Clojure',
     'Code quality',
     'Code review',
     'Compiler',
     'Continuous integration',
     'COVID-19',
     'C++']



### Extracting the description of each Topic



```python
# <p class="f5 color-fg-muted mb-0 mt-1">

# To get the description in order using the following tag 
selection_tag_desc = "f5 color-fg-muted mb-0 mt-1"
topic_description_tags  = doc.find_all('p' , {'class': selection_tag_desc })
len(topic_description_tags)
```




    30




```python
topic_description_tags[:2]
```




    [<p class="f5 color-fg-muted mb-0 mt-1">
               3D modeling is the process of virtually developing the surface and structure of a 3D object.
             </p>,
     <p class="f5 color-fg-muted mb-0 mt-1">
               Ajax is a technique for creating interactive web applications.
             </p>]




```python
topic_descriptions = [] 
for desc in topic_description_tags:
    topic_descriptions.append(desc.text.strip())
topic_descriptions
```




    ['3D modeling is the process of virtually developing the surface and structure of a 3D object.',
     'Ajax is a technique for creating interactive web applications.',
     'Algorithms are self-contained sequences that carry out a variety of tasks.',
     'Amp is a non-blocking concurrency library for PHP.',
     'Android is an operating system built by Google designed for mobile devices.',
     'Angular is an open source web application platform.',
     'Ansible is a simple and powerful automation engine.',
     'An API (Application Programming Interface) is a collection of protocols and subroutines for building software.',
     'Arduino is an open source hardware and software company and maker community.',
     'ASP.NET is a web framework for building modern web apps and services.',
     'Atom is a open source text editor built with web technologies.',
     'An awesome list is a list of awesome things curated by the community.',
     'Amazon Web Services provides on-demand cloud computing platforms on a subscription basis.',
     'Azure is a cloud computing service created by Microsoft.',
     'Babel is a compiler for writing next generation JavaScript, today.',
     'Bash is a shell and command language interpreter for the GNU operating system.',
     'Bitcoin is a cryptocurrency developed by Satoshi Nakamoto.',
     'Bootstrap is an HTML, CSS, and JavaScript framework.',
     'A bot is an application that runs automated tasks over the Internet.',
     'C is a general purpose programming language that first appeared in 1972.',
     'Chrome is a web browser from the tech company Google.',
     'Chrome extensions enable users to customize the Chrome browsing experience.',
     'A CLI, or command-line interface, is a console that helps users issue commands to a program.',
     'Clojure is a dynamic, general-purpose programming language.',
     'Automate your code review with style, quality, security, and test‑coverage checks when you need them.',
     'Ensure your code meets quality standards and ship with confidence.',
     'Compilers are software that translate higher-level programming languages to lower-level languages (e.g. machine code).',
     'Automatically build and test your code as you push it upstream, preventing bugs from being deployed to production.',
     'The coronavirus disease 2019 (COVID-19) is an infectious disease caused by SARS-CoV-2.',
     'C++ is a general purpose and object-oriented programming language.']



### Obtaining each topic's URL 


```python
#topic_title_tags0.parent
selection_tag_link = "no-underline flex-1 d-flex flex-column"
topic_link_tags = doc.find_all('a', {'class': selection_tag_link})
len(topic_link_tags)
```




    30




```python
# To get the Topic URL
topic_page_url = "https://github.com" + topic_link_tags[0]['href']
topic_page_url 
```




    'https://github.com/topics/3d'



Creating a List called `topic_urls`, which repea


```python
topic_urls = [] 
base_url = "https://github.com"
for url in topic_link_tags:
    topic_urls.append(base_url + url['href'])
topic_urls
```




    ['https://github.com/topics/3d',
     'https://github.com/topics/ajax',
     'https://github.com/topics/algorithm',
     'https://github.com/topics/amphp',
     'https://github.com/topics/android',
     'https://github.com/topics/angular',
     'https://github.com/topics/ansible',
     'https://github.com/topics/api',
     'https://github.com/topics/arduino',
     'https://github.com/topics/aspnet',
     'https://github.com/topics/atom',
     'https://github.com/topics/awesome',
     'https://github.com/topics/aws',
     'https://github.com/topics/azure',
     'https://github.com/topics/babel',
     'https://github.com/topics/bash',
     'https://github.com/topics/bitcoin',
     'https://github.com/topics/bootstrap',
     'https://github.com/topics/bot',
     'https://github.com/topics/c',
     'https://github.com/topics/chrome',
     'https://github.com/topics/chrome-extension',
     'https://github.com/topics/cli',
     'https://github.com/topics/clojure',
     'https://github.com/topics/code-quality',
     'https://github.com/topics/code-review',
     'https://github.com/topics/compiler',
     'https://github.com/topics/continuous-integration',
     'https://github.com/topics/covid-19',
     'https://github.com/topics/cpp']



### Importing Pandas to create a Dataframe 



```python
!pip install pandas --upgrade 
```

    Requirement already satisfied: pandas in /opt/conda/lib/python3.9/site-packages (1.3.3)
    Collecting pandas
      Downloading pandas-1.5.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (12.2 MB)
    [K     |████████████████████████████████| 12.2 MB 6.1 MB/s eta 0:00:01
    [?25hRequirement already satisfied: numpy>=1.20.3 in /opt/conda/lib/python3.9/site-packages (from pandas) (1.20.3)
    Requirement already satisfied: python-dateutil>=2.8.1 in /opt/conda/lib/python3.9/site-packages (from pandas) (2.8.2)
    Requirement already satisfied: pytz>=2020.1 in /opt/conda/lib/python3.9/site-packages (from pandas) (2021.1)
    Requirement already satisfied: six>=1.5 in /opt/conda/lib/python3.9/site-packages (from python-dateutil>=2.8.1->pandas) (1.16.0)
    Installing collected packages: pandas
      Attempting uninstall: pandas
        Found existing installation: pandas 1.3.3
        Uninstalling pandas-1.3.3:
          Successfully uninstalled pandas-1.3.3
    Successfully installed pandas-1.5.1



```python
import pandas as pd
```

#### Combining the Topic Title, Description and URL into a Dictionary


```python
topics_dict = {
    'title': topic_titles,
    'description': topic_descriptions,
    'url': topic_urls
}
```


```python
topics_df  = pd.DataFrame(topics_dict)
topics_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>description</th>
      <th>url</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3D</td>
      <td>3D modeling is the process of virtually develo...</td>
      <td>https://github.com/topics/3d</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ajax</td>
      <td>Ajax is a technique for creating interactive w...</td>
      <td>https://github.com/topics/ajax</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algorithm</td>
      <td>Algorithms are self-contained sequences that c...</td>
      <td>https://github.com/topics/algorithm</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Amp</td>
      <td>Amp is a non-blocking concurrency library for ...</td>
      <td>https://github.com/topics/amphp</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Android</td>
      <td>Android is an operating system built by Google...</td>
      <td>https://github.com/topics/android</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Angular</td>
      <td>Angular is an open source web application plat...</td>
      <td>https://github.com/topics/angular</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Ansible</td>
      <td>Ansible is a simple and powerful automation en...</td>
      <td>https://github.com/topics/ansible</td>
    </tr>
    <tr>
      <th>7</th>
      <td>API</td>
      <td>An API (Application Programming Interface) is ...</td>
      <td>https://github.com/topics/api</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Arduino</td>
      <td>Arduino is an open source hardware and softwar...</td>
      <td>https://github.com/topics/arduino</td>
    </tr>
    <tr>
      <th>9</th>
      <td>ASP.NET</td>
      <td>ASP.NET is a web framework for building modern...</td>
      <td>https://github.com/topics/aspnet</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Atom</td>
      <td>Atom is a open source text editor built with w...</td>
      <td>https://github.com/topics/atom</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Awesome Lists</td>
      <td>An awesome list is a list of awesome things cu...</td>
      <td>https://github.com/topics/awesome</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Amazon Web Services</td>
      <td>Amazon Web Services provides on-demand cloud c...</td>
      <td>https://github.com/topics/aws</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Azure</td>
      <td>Azure is a cloud computing service created by ...</td>
      <td>https://github.com/topics/azure</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Babel</td>
      <td>Babel is a compiler for writing next generatio...</td>
      <td>https://github.com/topics/babel</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Bash</td>
      <td>Bash is a shell and command language interpret...</td>
      <td>https://github.com/topics/bash</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Bitcoin</td>
      <td>Bitcoin is a cryptocurrency developed by Satos...</td>
      <td>https://github.com/topics/bitcoin</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Bootstrap</td>
      <td>Bootstrap is an HTML, CSS, and JavaScript fram...</td>
      <td>https://github.com/topics/bootstrap</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Bot</td>
      <td>A bot is an application that runs automated ta...</td>
      <td>https://github.com/topics/bot</td>
    </tr>
    <tr>
      <th>19</th>
      <td>C</td>
      <td>C is a general purpose programming language th...</td>
      <td>https://github.com/topics/c</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Chrome</td>
      <td>Chrome is a web browser from the tech company ...</td>
      <td>https://github.com/topics/chrome</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Chrome extension</td>
      <td>Chrome extensions enable users to customize th...</td>
      <td>https://github.com/topics/chrome-extension</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Command line interface</td>
      <td>A CLI, or command-line interface, is a console...</td>
      <td>https://github.com/topics/cli</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Clojure</td>
      <td>Clojure is a dynamic, general-purpose programm...</td>
      <td>https://github.com/topics/clojure</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Code quality</td>
      <td>Automate your code review with style, quality,...</td>
      <td>https://github.com/topics/code-quality</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Code review</td>
      <td>Ensure your code meets quality standards and s...</td>
      <td>https://github.com/topics/code-review</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Compiler</td>
      <td>Compilers are software that translate higher-l...</td>
      <td>https://github.com/topics/compiler</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Continuous integration</td>
      <td>Automatically build and test your code as you ...</td>
      <td>https://github.com/topics/continuous-integration</td>
    </tr>
    <tr>
      <th>28</th>
      <td>COVID-19</td>
      <td>The coronavirus disease 2019 (COVID-19) is an ...</td>
      <td>https://github.com/topics/covid-19</td>
    </tr>
    <tr>
      <th>29</th>
      <td>C++</td>
      <td>C++ is a general purpose and object-oriented p...</td>
      <td>https://github.com/topics/cpp</td>
    </tr>
  </tbody>
</table>
</div>



## Getting Information from a Topic Page


```python
# Requesting a response from the Topic URL
response = requests.get(topic_page_url)
topic_page_url 
```




    'https://github.com/topics/3d'




```python
response.status_code
```




    200




```python
# No of characters in the website
len(response.text)
```




    451116




```python
# Using HTML Parser from BeautifulSoup
topic_doc = BeautifulSoup(response.text, 'html.parser')

h3_selection_class = "f3 color-fg-muted text-normal lh-condensed"
repo_tags = topic_doc.find_all('h3', {'class': h3_selection_class})
len(repo_tags)
```




    20




```python
# Obtaining all the information present in all the a-tags throughout the website
a_tags = repo_tags[0].find_all('a')
```

![image.png](attachment:image.png)


```python
# Obtaining the author's name of the Repository - First
a_tags[0].text.strip()
```




    'mrdoob'




```python
# Obtaining the author's name of the Repository - Second
a_tags[1].text.strip()
```




    'three.js'




```python
# Repository link of the Author 
base_url = "https://github.com"
repo_url = base_url + a_tags[1]['href']
print(repo_url)
```

    https://github.com/mrdoob/three.js



```python
# Finding the number of Stars for all Repositories
star_tags = topic_doc.find_all('span', {'class' : "Counter js-social-count"})
len(star_tags)
```




    20




```python
star_tags[0].text.strip() 
```




    '87k'




```python
# This function converts the 'k' into 1000
# For instance 87k will be converted into 87000

def parse_star_count(stars_str):
    stars_str = stars_str.strip()
    if stars_str[-1] == 'k':
        return int(float(stars_str[:-1])*1000)
    return int(stars_str)
```


```python
parse_star_count(star_tags[0].text.strip())
```




    87000




```python
def get_repo_info(h1_tag, stars_tag):
    #returns the required information of a repository 
    a_tags = h1_tag.find_all('a')
    username =  a_tags[0].text.strip()
    repo_name = a_tags[1].text.strip()
    repo_url = base_url + a_tags[1]['href']
    stars = parse_star_count(stars_tag.text.strip())
    return username, repo_name, stars, repo_url 
```


```python
get_repo_info(repo_tags[0], star_tags[0])
```




    ('mrdoob', 'three.js', 87000, 'https://github.com/mrdoob/three.js')




```python
# Repeating the above steps to get the Author's name, repository name, stars and repository url.
# Feeding this information into a Dictonary 

topic_repos_dict = {
    'username':[],
    'repo_name':[],
    'stars':[],
    'repo_url':[]
}

for i in range(len(repo_tags)):
    repo_info = get_repo_info(repo_tags[i], star_tags[i])
    topic_repos_dict['username'].append(repo_info[0]),
    topic_repos_dict['repo_name'].append(repo_info[1]),
    topic_repos_dict['stars'].append(repo_info[2]),
    topic_repos_dict['repo_url'].append(repo_info[3])
```


```python
topic_repos_df = pd.DataFrame(topic_repos_dict)
topic_repos_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>username</th>
      <th>repo_name</th>
      <th>stars</th>
      <th>repo_url</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>mrdoob</td>
      <td>three.js</td>
      <td>87000</td>
      <td>https://github.com/mrdoob/three.js</td>
    </tr>
    <tr>
      <th>1</th>
      <td>libgdx</td>
      <td>libgdx</td>
      <td>20800</td>
      <td>https://github.com/libgdx/libgdx</td>
    </tr>
    <tr>
      <th>2</th>
      <td>pmndrs</td>
      <td>react-three-fiber</td>
      <td>20400</td>
      <td>https://github.com/pmndrs/react-three-fiber</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BabylonJS</td>
      <td>Babylon.js</td>
      <td>18800</td>
      <td>https://github.com/BabylonJS/Babylon.js</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ssloy</td>
      <td>tinyrenderer</td>
      <td>15200</td>
      <td>https://github.com/ssloy/tinyrenderer</td>
    </tr>
    <tr>
      <th>5</th>
      <td>aframevr</td>
      <td>aframe</td>
      <td>14800</td>
      <td>https://github.com/aframevr/aframe</td>
    </tr>
    <tr>
      <th>6</th>
      <td>lettier</td>
      <td>3d-game-shaders-for-beginners</td>
      <td>14000</td>
      <td>https://github.com/lettier/3d-game-shaders-for...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>FreeCAD</td>
      <td>FreeCAD</td>
      <td>12600</td>
      <td>https://github.com/FreeCAD/FreeCAD</td>
    </tr>
    <tr>
      <th>8</th>
      <td>CesiumGS</td>
      <td>cesium</td>
      <td>9600</td>
      <td>https://github.com/CesiumGS/cesium</td>
    </tr>
    <tr>
      <th>9</th>
      <td>metafizzy</td>
      <td>zdog</td>
      <td>9500</td>
      <td>https://github.com/metafizzy/zdog</td>
    </tr>
    <tr>
      <th>10</th>
      <td>timzhang642</td>
      <td>3D-Machine-Learning</td>
      <td>8500</td>
      <td>https://github.com/timzhang642/3D-Machine-Lear...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>isl-org</td>
      <td>Open3D</td>
      <td>7700</td>
      <td>https://github.com/isl-org/Open3D</td>
    </tr>
    <tr>
      <th>12</th>
      <td>a1studmuffin</td>
      <td>SpaceshipGenerator</td>
      <td>7200</td>
      <td>https://github.com/a1studmuffin/SpaceshipGener...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>blender</td>
      <td>blender</td>
      <td>7100</td>
      <td>https://github.com/blender/blender</td>
    </tr>
    <tr>
      <th>14</th>
      <td>domlysz</td>
      <td>BlenderGIS</td>
      <td>5800</td>
      <td>https://github.com/domlysz/BlenderGIS</td>
    </tr>
    <tr>
      <th>15</th>
      <td>FyroxEngine</td>
      <td>Fyrox</td>
      <td>5200</td>
      <td>https://github.com/FyroxEngine/Fyrox</td>
    </tr>
    <tr>
      <th>16</th>
      <td>openscad</td>
      <td>openscad</td>
      <td>5100</td>
      <td>https://github.com/openscad/openscad</td>
    </tr>
    <tr>
      <th>17</th>
      <td>spritejs</td>
      <td>spritejs</td>
      <td>5000</td>
      <td>https://github.com/spritejs/spritejs</td>
    </tr>
    <tr>
      <th>18</th>
      <td>google</td>
      <td>model-viewer</td>
      <td>5000</td>
      <td>https://github.com/google/model-viewer</td>
    </tr>
    <tr>
      <th>19</th>
      <td>jagenjo</td>
      <td>webglstudio.js</td>
      <td>4800</td>
      <td>https://github.com/jagenjo/webglstudio.js</td>
    </tr>
  </tbody>
</table>
</div>




```python

```

# Final Code 


```python
 def get_topic_page(topic_url):
    #Download the page 
    response = requests.get(topic_url)
    
    # check for succuessful response 
    if response.status_code != 200:
        raise Exception("Failed to load page {}".format(topic_url))
    #Parse Using BeautifulSoup 
    topic_doc = BeautifulSoup(response.text, 'html.parser')
    return topic_doc

def get_repo_info(h1_tag, stars_tag):
    #returns the required information of a repository 
    a_tags = h1_tag.find_all('a')
    username =  a_tags[0].text.strip()
    repo_name = a_tags[1].text.strip()
    repo_url = base_url + a_tags[1]['href']
    stars = parse_star_count(stars_tag.text.strip())
    return username, repo_name, stars, repo_url 

def get_topic_repos(topic_doc):
    
    #Get the h3 Tags containing repo title, repo URL and username
    h3_selection_class = "f3 color-fg-muted text-normal lh-condensed"
    repo_tags = topic_doc.find_all('h3', {'class': h3_selection_class})
    
    #Get Star Tags 
    star_tags = topic_doc.find_all('span', {'class' : "Counter js-social-count"})
    
    topic_repos_dict = {
    'username':[],
    'repo_name':[],
    'stars':[],
    'repo_url':[]
    }

    for i in range(len(repo_tags)):
        repo_info = get_repo_info(repo_tags[i], star_tags[i])
        topic_repos_dict['username'].append(repo_info[0]),
        topic_repos_dict['repo_name'].append(repo_info[1]),
        topic_repos_dict['stars'].append(repo_info[2]),
        topic_repos_dict['repo_url'].append(repo_info[3])
    return pd.DataFrame(topic_repos_dict)


```


```python
def get_topic_titles(doc):
    #Selection of all Title Tags 
    selection_class = "f3 lh-condensed mb-0 mt-1 Link--primary"
    topic_title_tags = doc.find_all('p', {'class': selection_class})
    
    topic_titles = [] 
    for tag in topic_title_tags:
        topic_titles.append(tag.text)
    return topic_titles

def get_topic_descriptions(doc):
    #Selection of all Description 
    selection_tag_desc = "f5 color-fg-muted mb-0 mt-1"
    topic_description_tags  = doc.find_all('p' , {'class': selection_tag_desc })
    
    topic_descriptions = [] 
    for desc in topic_description_tags:
        topic_descriptions.append(desc.text.strip())
    return topic_descriptions

def get_topic_urls(doc):
    #Selection of all Links 
    selection_tag_link = "no-underline flex-1 d-flex flex-column"
    topic_link_tags = doc.find_all('a', {'class': selection_tag_link})
    
    topic_urls = [] 
    base_url = "https://github.com"
    for url in topic_link_tags:
        topic_urls.append(base_url + url['href'])
    return topic_urls

def scrape_topics():
    topics_url = "https://github.com/topics"
    response =  requests.get(topics_url)
    if response.status_code != 200:
        raise Exception("Failed to load page {}".format(topic_url))

    topics_dict = {
        'title': get_topic_titles(doc),
        'description': get_topic_descriptions(doc),
        'url': get_topic_urls(doc)
    }
    topics_df  = pd.DataFrame(topics_dict)
    return topics_df


    
```


```python
topic_repos_df = pd.DataFrame(topic_repos_dict)
topic_repos_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>username</th>
      <th>repo_name</th>
      <th>stars</th>
      <th>repo_url</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>mrdoob</td>
      <td>three.js</td>
      <td>87000</td>
      <td>https://github.com/mrdoob/three.js</td>
    </tr>
    <tr>
      <th>1</th>
      <td>libgdx</td>
      <td>libgdx</td>
      <td>20800</td>
      <td>https://github.com/libgdx/libgdx</td>
    </tr>
    <tr>
      <th>2</th>
      <td>pmndrs</td>
      <td>react-three-fiber</td>
      <td>20400</td>
      <td>https://github.com/pmndrs/react-three-fiber</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BabylonJS</td>
      <td>Babylon.js</td>
      <td>18800</td>
      <td>https://github.com/BabylonJS/Babylon.js</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ssloy</td>
      <td>tinyrenderer</td>
      <td>15200</td>
      <td>https://github.com/ssloy/tinyrenderer</td>
    </tr>
    <tr>
      <th>5</th>
      <td>aframevr</td>
      <td>aframe</td>
      <td>14800</td>
      <td>https://github.com/aframevr/aframe</td>
    </tr>
    <tr>
      <th>6</th>
      <td>lettier</td>
      <td>3d-game-shaders-for-beginners</td>
      <td>14000</td>
      <td>https://github.com/lettier/3d-game-shaders-for...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>FreeCAD</td>
      <td>FreeCAD</td>
      <td>12600</td>
      <td>https://github.com/FreeCAD/FreeCAD</td>
    </tr>
    <tr>
      <th>8</th>
      <td>CesiumGS</td>
      <td>cesium</td>
      <td>9600</td>
      <td>https://github.com/CesiumGS/cesium</td>
    </tr>
    <tr>
      <th>9</th>
      <td>metafizzy</td>
      <td>zdog</td>
      <td>9500</td>
      <td>https://github.com/metafizzy/zdog</td>
    </tr>
    <tr>
      <th>10</th>
      <td>timzhang642</td>
      <td>3D-Machine-Learning</td>
      <td>8500</td>
      <td>https://github.com/timzhang642/3D-Machine-Lear...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>isl-org</td>
      <td>Open3D</td>
      <td>7700</td>
      <td>https://github.com/isl-org/Open3D</td>
    </tr>
    <tr>
      <th>12</th>
      <td>a1studmuffin</td>
      <td>SpaceshipGenerator</td>
      <td>7200</td>
      <td>https://github.com/a1studmuffin/SpaceshipGener...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>blender</td>
      <td>blender</td>
      <td>7100</td>
      <td>https://github.com/blender/blender</td>
    </tr>
    <tr>
      <th>14</th>
      <td>domlysz</td>
      <td>BlenderGIS</td>
      <td>5800</td>
      <td>https://github.com/domlysz/BlenderGIS</td>
    </tr>
    <tr>
      <th>15</th>
      <td>FyroxEngine</td>
      <td>Fyrox</td>
      <td>5200</td>
      <td>https://github.com/FyroxEngine/Fyrox</td>
    </tr>
    <tr>
      <th>16</th>
      <td>openscad</td>
      <td>openscad</td>
      <td>5100</td>
      <td>https://github.com/openscad/openscad</td>
    </tr>
    <tr>
      <th>17</th>
      <td>spritejs</td>
      <td>spritejs</td>
      <td>5000</td>
      <td>https://github.com/spritejs/spritejs</td>
    </tr>
    <tr>
      <th>18</th>
      <td>google</td>
      <td>model-viewer</td>
      <td>5000</td>
      <td>https://github.com/google/model-viewer</td>
    </tr>
    <tr>
      <th>19</th>
      <td>jagenjo</td>
      <td>webglstudio.js</td>
      <td>4800</td>
      <td>https://github.com/jagenjo/webglstudio.js</td>
    </tr>
  </tbody>
</table>
</div>




```python
def get_topic_page(topic_url):
    #Download the page 
    response = requests.get(topic_url)
    
    # check for succuessful response 
    if response.status_code != 200:
        raise Exception("Failed to load page {}".format(topic_url))
    #Parse Using BeautifulSoup 
    topic_doc = BeautifulSoup(response.text, 'html.parser')
    return topic_doc

def get_repo_info(h1_tag, stars_tag):
    #returns the required information of a repository 
    a_tags = h1_tag.find_all('a')
    username =  a_tags[0].text.strip()
    repo_name = a_tags[1].text.strip()
    repo_url = base_url + a_tags[1]['href']
    stars = parse_star_count(stars_tag.text.strip())
    return username, repo_name, stars, repo_url 

def get_topic_repos(topic_doc):
    
    #Get the h3 Tags containing repo title, repo URL and username
    h3_selection_class = "f3 color-fg-muted text-normal lh-condensed"
    repo_tags = topic_doc.find_all('h3', {'class': h3_selection_class})
    
    #Get Star Tags 
    star_tags = topic_doc.find_all('span', {'class' : "Counter js-social-count"})
    
    topic_repos_dict = {
    'username':[],
    'repo_name':[],
    'stars':[],
    'repo_url':[]
    }

    for i in range(len(repo_tags)):
        repo_info = get_repo_info(repo_tags[i], star_tags[i])
        topic_repos_dict['username'].append(repo_info[0]),
        topic_repos_dict['repo_name'].append(repo_info[1]),
        topic_repos_dict['stars'].append(repo_info[2]),
        topic_repos_dict['repo_url'].append(repo_info[3])
    return pd.DataFrame(topic_repos_dict)

def scrape_topic(topic_url, path):
    if os.path.exists(path):
        print("The file {} already exists. Skipping ...".format(path))
        return
    topic_df = get_topic_repos(get_topic_page(topic_url))
    topic_df.to_csv(path + '.csv', index = None)
```


```python
import os 
```


```python
def scrape_topics_repos():
    topics_df = scrape_topics()
    
    #creating a folder 
    os.makedirs('Data', exist_ok=True)
    
    for index, row in topics_df.iterrows():
        print('Scraping Top Repositories for "{}"'.format(row['title']))
        scrape_topic(row['url'], 'Data/{}'.format(row['title']))
```


```python
scrape_topics_repos()
```

    Scraping Top Repositories for "3D"
    Scraping Top Repositories for "Ajax"
    Scraping Top Repositories for "Algorithm"
    Scraping Top Repositories for "Amp"
    Scraping Top Repositories for "Android"
    Scraping Top Repositories for "Angular"
    Scraping Top Repositories for "Ansible"
    Scraping Top Repositories for "API"
    Scraping Top Repositories for "Arduino"
    Scraping Top Repositories for "ASP.NET"
    Scraping Top Repositories for "Atom"
    Scraping Top Repositories for "Awesome Lists"
    Scraping Top Repositories for "Amazon Web Services"
    Scraping Top Repositories for "Azure"
    Scraping Top Repositories for "Babel"
    Scraping Top Repositories for "Bash"
    Scraping Top Repositories for "Bitcoin"
    Scraping Top Repositories for "Bootstrap"
    Scraping Top Repositories for "Bot"
    Scraping Top Repositories for "C"
    Scraping Top Repositories for "Chrome"
    Scraping Top Repositories for "Chrome extension"
    Scraping Top Repositories for "Command line interface"
    Scraping Top Repositories for "Clojure"
    Scraping Top Repositories for "Code quality"
    Scraping Top Repositories for "Code review"
    Scraping Top Repositories for "Compiler"
    Scraping Top Repositories for "Continuous integration"
    Scraping Top Repositories for "COVID-19"
    Scraping Top Repositories for "C++"



```python

```
