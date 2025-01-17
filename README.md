# This repository is DEPRECATED

The [graphsense-tagpack tool](https://github.com/graphsense/graphsense-tagpack-tool), starting from **version 23.03**, ships with inbuilt taxonomies (a subset of the Interpol definitions). If you want to use the graphsense default taxonomies use these built-in ones. If you need special taxonomies the tagpack tool still provides the function to load your own taxonomies via a config file.

If you need the entire taxonomy the Interpol repository this repository was forked from (https://github.com/INTERPOL-Innovation-Centre/DW-VA-Taxonomy) direcly.

# Dark Web and Virtual Assets Taxonomies

This repository contains taxonomies covering common types of entities found in Dark Web and Virtual Assets ecosystems, as well as abusive, possibly criminal behaviors found in real-world investigations.

Taxonomies are defined as [YAML][yaml] files and automatically rendered as a human-readable Website by using the [Jekyll][jekyll] static site generator: https://graphsense.github.io/DW-VA-Taxonomy/

## How to run the Website locally

Checkout this git repository to your local machine

	git clone git@github.com:graphsense/DW-VA-Taxonomy.git
	cd DW-VA-Taxonomy

Install ruby as described [here](https://jekyllrb.com/docs/installation/macos/)

	brew install chruby ruby-install
	ruby-install ruby

	echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
	echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc
	echo "chruby ruby-3.1.2" >> ~/.zshrc

	ruby -v

Install Jekyll

	gem install jekyll bundler

Install local bundles

	bundle install
	bundle add webrick
	
Run website in local development mode

	bundle exec jekyll serve --watch

Website should run in http://127.0.0.1:4000/DW-VA-Taxonomy/

## How to create / modify / delete concept definitions

All taxonomy definition YAML-files reside in the [\_data](_data) directory.

Each taxonomy file defines a so-called [(concept) **scheme**](https://www.w3.org/2009/08/skos-reference/skos.html#ConceptScheme), which can be viewed as an aggregation of one or more [SKOS][skos] concepts.

	scheme:
	  baseurl: /taxonomies/entities
	  csvfile: assets/data/entities.csv
	  type: conceptScheme
	  version: 0.1
	  lastmod: 2020-03-31
	  title: Entity Taxonomy
	  creator: INTERPOL Darknet and Cryptocurrencies Working Group
	  description: >
	   This taxonomy defines entities that represent real-world actors
	   and service that are part of a larger Darknet- and Cryptoasset Ecosystems.
	  default_language: en

Further it defines a number of concepts that are linked with each other through [broader](https://www.w3.org/2009/08/skos-reference/skos.html#broader)
and [narrower](https://www.w3.org/2009/08/skos-reference/skos.html#narrower) relations and therefore form a hierarchical structure. The `seeAlso`relation can be used for referencing external definition sources.

	exchange:
	  id: exchange
	  type: concept
	  prefLabel: Exchange
	  description: >
	    A cryptocurrency exchange or a digital currency exchange (DCE)
	    is a business that allows customers to trade cryptocurrencies or
	    digital currencies for other assets, such as conventional fiat
	    money or other digital currencies.
	  seeAlso: https://en.wikipedia.org/wiki/Cryptocurrency_exchange
	  broader: service

A taxonomy can be modified by changing these definitions.

## How to generate downloadable CSV files from defined taxonomies

The taxonomy YAML files in `_data` feed the [Jekyll static Website generator][jekyll] but they are not directly exposed on the Web.

In order to provide machine-processable, downloadable taxonomy representations, they must first be converted into (a) commonly accepted format(s) and provided as downloadable files. This is what the conversion script `script/convert.py` does.

Before running the script, make sure you have Python 3 up and running and install the dependencies using [conda](https://docs.conda.io/en/latest/).

	conda env create -f environment.yml

Then run the conversion script by passing the taxonomy YAML files and an optional output folder (which is `assets/data` by default.

	./scripts/convert.py -t _data/*.yaml -d assets/data

This converts each YAML file into a corresponding CSV file, which can then be integrated by client applications that make use of that taxonomy.

	id,uri,label,description
	exchange,https://interpol-innovation-centre.github.io/DW-VA-Taxonomy/taxonomies/entities#exchange,Exchange,"A cryptocurrency exchange or a digital currency exchange (DCE) is a business that allows customers to trade cryptocurrencies or digital currencies for other assets, such as conventional fiat money or other digital currencies. A cryptocurrency exchange can be a market maker that typically takes the bid-ask spreads as a transaction commission for is service or, as a matching platform, simply charges fees."

After conversion, these files must be added and committed in order to show up on the Website.

	git add assets/data
	git commit -m "updated taxonomy serialization"

## How to contribute

If you have write access to this repository, create a branch, change stuff, and push the branch to GitHub.

	git branch my_concept_proposal
	git checkout my_concept_proposal
	... change files ..
	git add [... changed files ...]
	git commit -m "some meaningful message"
	git push -u origin my_concept_proposal

Please note that the **master branch is protected**. It feeds the static Website generator and should therefore not be modified without testing things first.

If you are an **external contributor** without direct write access to this repository, then you can basically follow the same procedure, with the main difference being that **you must fork this repository** before. Please check to the [forking documentation](https://guides.github.com/activities/forking/) on Github.

Should you wish to contribute to this project, please review our [Contributor's page][contributing].

## FAQ

**Why are taxonomies represented in YAML and not in a widely accept SKOS serialization format?**

Well, mostly for technical and usability reasons. Jekyll can read and parse YAML files and also people with minimal dev skills can most likely create and/or modify a YAML file. RDF and other formats certainly have a higher entry barrier. However, our taxonomies follow the SKOS standard and can therefore be easily converted. If required, we can do this in future releases.


[contributing]: CONTRIBUTING.md
[yaml]: https://yaml.org/
[jekyll]: https://jekyllrb.com/
[python]: https://www.python.org/
[skos]: https://www.w3.org/2004/02/skos/
[docker]: https://www.docker.com/
