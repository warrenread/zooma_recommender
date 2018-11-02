## zooma_recommender install and run

Ensure you have Python 3.x, including pip (i.e. pip3)

Do you have the `virtualenv` python package?

```bash
pycheck() { \
    [ "$#" -eq 1 ] && \
    ( python3 -c "import $1" 2> /dev/null && echo "$1 already installed" || \
    echo "please install $1" ) || \
    ( echo "${FUNCNAME[0]} requires a single argument" ); \
}
pycheck virtualenv
```

If it's not there:

```bash
pip3 install virtualenv
```

Now create, activate and install necessary packages into a virtual environment
for `zooma_recommender`, under your checked-out `zooma-recommender` code
directory. For the sake of Git, it will help if you call your environment
"zenv", i.e.:

```bash
cd zooma_recommender
virtualenv zenv
. ./zenv/bin/activate
pip3 install -r requirements.txt
```

Now run the Python script to generate rules.txt:

```bash
pyrun() { chmod a+x $1; ./$1; }
pyrun efficient_apriori_pdx.py
```

Install Solr 5.5.3

Start solr in schemaless mode

```bash
./bin/solr start -e schemaless
```

Create a new Solr core

```bash
./bin/solr create -c zooma_rules
```

Load the rules

```bash
./bin/post -p 8983 -c zooma_rules ../rules.json
```

Run a query

```
(properties.propertyType:(tumortype AND origintissue AND samplediagnosis)) AND
 (properties.propertyValue: Blood AND
 properties.propertyValue: "acute myeloid leukemia" AND
 properties.propertyValue: "Primary") AND
 tag.propertyType : samplediagnosis
```
