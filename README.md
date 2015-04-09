scholarly.py
===========

scholarly.py is a module that allows you to retrieve author and publication information from [Google Scholar](https://scholar.google.com) in a friendly, Pythonic way.


Usage
-----
Because `scholarly` does not use an official API, no key is required. Simply:

```python
import scholarly

print scholarly.search_author('Steven A. Cholewiak').next()
```

### Methods
* `search_author` -- Search for an author by name and return a generator of Author objects.

```python
    >>> search_query = scholarly.search_author('Manish Singh')
    >>> print search_query.next()
    {'_filled': False,
     'affiliation': u'Rutgers University, New Brunswick, NJ',
     'citedby': 2283,
     'email': u'@ruccs.rutgers.edu',
     'id': '9XRvM88AAAAJ',
     'interests': [u'Human perception',
                   u'Computational Vision',
                   u'Cognitive Science'],
     'name': u'Manish Singh',
     'url_citations': '/citations?user=9XRvM88AAAAJ&hl=en',
     'url_picture': '/citations/images/avatar_scholar_150.jpg'}
```

* `search_keyword` -- Search by keyword and return a generator of Author objects.

```python
    >>> search_query = scholarly.search_keyword('Haptics')
    >>> print search_query.next()
    {'_filled': False,
     'affiliation': u'Stanford University',
     'citedby': 18625,
     'email': u'@cs.stanford.edu',
     'id': '4arkOLcAAAAJ',
     'interests': [u'Robotics', u'Haptics', u'Human Motion'],
     'name': u'Oussama Khatib',
     'url_citations': '/citations?user=4arkOLcAAAAJ&hl=en',
     'url_picture': '/citations/images/avatar_scholar_150.jpg'}
```

* `search_pubs_query` -- Search for articles/publications and return generator of Publication objects.

```python
    >>> search_query = scholarly.search_pubs_query('Perception of physical stability and center of mass of 3D objects')
    >>> print search_query.next()
    {'_filled': False,
     'bib': {'abstract': u'Humans can judge from vision alone whether an object is physically stable or not. Such judgments allow observers to predict the physical behavior of objects, and hence to guide their motor actions. We investigated the visual estimation of physical stability of 3-D  ...',
         'author': u'SA Cholewiak and RW Fleming and M Singh',
         'eprint': u'http://www.journalofvision.org/content/15/2/13.full',
         'title': u'Perception of physical stability and center of mass of 3-D objects',
         'url': u'http://www.journalofvision.org/content/15/2/13.short'},
     'source': 'scholar',
     'url_scholarbib': u'/scholar.bib?q=info:K8ZpoI6hZNoJ:scholar.google.com/&output=citation&hl=en&ct=citation&cd=0'}
```


### Example
Here's a quick example demonstrating how to retrieve an author's profile then retrieve the titles of the papers that cite his most popular (cited) paper.

```python
    >>> # Retrieve the author's data, fill-in, and print
    >>> search_query = scholarly.search_author('Steven A Cholewiak')
    >>> author = search_query.next().fill()
    >>> print author

    >>> # Print the titles of the author's publications
    >>> print [pub.bib['title'] for pub in author.publications]

    >>> # Take a closer look at the first publication
    >>> pub = author.publications[0].fill()
    >>> print pub

    >>> # Which papers cited that publication?
    >>> print [citation.bib['title'] for citation in pub.citedby()]
```


Installation
------------
Use `pip`:

    pip install scholarly

Or clone the package:

    git clone https://github.com/OrganicIrradiation/scholarly.git


Requirements
------------

Requires [bibtexparser](https://pypi.python.org/pypi/bibtexparser/), [Beautiful Soup](https://pypi.python.org/pypi/beautifulsoup4/), and [python-dateutil](https://pypi.python.org/pypi/python-dateutil/).


Changes
-------

Note that because of the nature of web scraping, this project will be in **perpetual alpha**.

### v0.1.3

* Raise an exception when we receive a Bot Check. Reorganized test.py alphabetically and updated its test cases. Reorganized README. Added python-dateutil as installation requirement, for some reason it was accidentally omitted.

### v0.1.2

* Now request HTTPS connection rather than HTTP and update test.py to account for a new "Zucker".  Also added information for the v0.1.1 revision.

### v0.1.1

* Fixed an issue with multi-page Author results, author entries with no citations (which are rare, but do occur), and added some tests using unittest.

### v0.1

* Initial release.


License
-------

The original code that this project was forked from was released by [Bello Chalmers](https://github.com/lbello/chalmers-web) under a [WTFPL](http://www.wtfpl.net/) license. In keeping with this mentality, all code is released under the [Unlicense](http://unlicense.org/).