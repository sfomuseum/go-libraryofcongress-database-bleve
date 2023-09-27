# go-libraryofcongress-database-bleve

Go package implementing the `sfomuseum/go-libraryofcongress-database.LibraryOfCongressDatabase` interface for use with Bleve.

## Documentation

Documentation is incomplete at this time.

## Tools

```
$> make cli
go build -mod vendor -ldflags="-s -w" -o bin/server cmd/server/main.go
go build -mod vendor -ldflags="-s -w" -o bin/query cmd/query/main.go
go build -mod vendor -ldflags="-s -w" -o bin/index cmd/index/main.go
```

### index

```
$> /bin/parse-lcsh https://id.loc.gov/download/lcsh.both.ndjson.zip | \
	./bin/index -database-uri bleve:///usr/local/data/lcsh.db -lcsh-data -
	
processed 5239 records in 1m0.000080042s (started 2023-09-27 12:17:25.580496 -0700 PDT m=+0.042965668)
processed 11080 records in 2m0.000402375s (started 2023-09-27 12:17:25.580496 -0700 PDT m=+0.042965668)

... time passes

processed 451737 records in 1h18m0.000995417s (started 2023-09-27 12:17:25.580496 -0700 PDT m=+0.042965668)
processed 457565 records in 1h19m0.000987917s (started 2023-09-27 12:17:25.580496 -0700 PDT m=+0.042965668)
```

Bleve is not the fastest database to index so keep that in mind especially if you're planning to index the Named Authority File (LCNAF) data which contains 11M+ records. Also keep in mind that a Bleve database containing 450,000 records takes up 571MB of disk space so you can extrapolate how large a database containing LCNAF records would be.

```
$> du -h /usr/local/data/lcsh.db/
571M	/usr/local/data/lcsh.db/
```

### query

```
$> ./bin/query -database-uri bleve:///usr/local/data/lcsh.db Montreal

lcsh:sh85087079 Montreal River (Ont.)
lcsh:sh2010014761 Alfa Romeo Montreal automobile
lcsh:sh2017003022 Montreal Massacre, Montréal, Québec, 1989
```

## See also

* https://github.com/sfomuseum/go-libraryofcongress
* https://github.com/sfomuseum/go-libraryofcongress-database
* https://blevesearch.com/