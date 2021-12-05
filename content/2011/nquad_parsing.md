Title: NQuad parsing using Jython
Date: 2021-12-05 16:00
Category: Semantic Web
Tags: semantic web, software development
Slug: nquad_parsing_jython
Authors: Uldis
Summary: Using NxParser for parsing NQuad RDF data from Jython.

When in need to parse NQuad RDF files (e.g., the Billion Triples Challenge data files) Java folks can use the NxParser by Aidan Hogan and Andreas Harth: [NxParser - Parser for NTriples, NQuads, and more](https://web.archive.org/web/20120725174828/http://sw.deri.org/2006/08/nxparser/).

You can also use it from Python (provided you use the Jython implementation).

Code:

```
import sys
	sys.path.append("./nxparser.jar")
	 
	from org.semanticweb.yars.nx.parser import *
	from java.io import FileInputStream
	from java.util.zip import GZIPInputStream
	 
	def all_triples(fname, use_gzip=False):
	    in_file = FileInputStream(fname)
	    if use_gzip:
	        in_file = GZIPInputStream(in_file)
	 
	    nxp = NxParser(in_file, False)
	 
	    while nxp.hasNext():
	        triple = nxp.next()
	        n3 = ([i.toN3() for i in triple])
	        yield n3
```

The code above defines a generator function which will yield a stream of NQuad records. We can now add some demo code in order to see it in action:

Code:

```
def main():
	    gzfname = "sioc-btc-2009.gz"
	 
	    for line in all_triples(gzfname, use_gzip=True):
	        print line
	 
	if __name__ == "__main__":
	    main()
```

Code:

```
	[u'<http://2008.blogtalk.net/node/29>', u'<http://www.w3.org/1999/02/22-rdf-syntax-ns#type>', u'<http://rdfs.org/sioc/ns#Post>', u'<http://2008.blogtalk.net/sioc/node/29>']
	 
	[u'<http://2008.blogtalk.net/node/65>', u'<http://rdfs.org/sioc/ns#content>', u'"We\'ve created a map showing the main places of interest (event locations, restaurants, pubs, shopping locations and tourist sights) during BlogTalk 2008.  The conference venue is shown on the left-hand side of the map.  We will also have a hardcopy for all attendees. View Larger Map"', u'<http://2008.blogtalk.net/sioc/node/65>']
```

Notes:
  - I was using this code to parse BTC data files.
  - NQuad parsing was recently added to [Redland](http://librdf.org).

*Hmmm. Markdown lists are not working for me here. How can I solve this?*


