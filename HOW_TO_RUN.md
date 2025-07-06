## Run with Jacoco Agent

To build the application:
```
mvn package -DskipTests
```

First download Jacoco at `https://www.eclemma.org/jacoco/`. Actually we only need `jacocoagent.jar` and `jacococli.jar` in `lib`.

This application relies on a database, which is available at [heidelberg.oshdb](https://downloads.ohsome.org/OSHDB/v1.0/europe/germany/baden-wuerttemberg/heidelberg.oshdb.mv.db). I downloaded a copy on Mar 29, 2023: [My copy](https://gtvault-my.sharepoint.com/:u:/g/personal/rhuang329_gatech_edu/EUgs3e4DjwRPkKT89tFdI9sBXkTKQw0OFVIXqy3Ta6R16g?e=EbViuV)

Run the application with Jacoco Agent with this command. Replace `/home/rkh/Downloads/share/lib/jacocoagent.jar` with your `jacocoagent.jar` and `6300` is the Jacoco agent's port. Provide `db=` with your local database copy.
```
java -Djdk.attach.allowAttachSelf=true -javaagent:/home/rkh/Downloads/share/lib/jacocoagent.jar=output=tcpserver,port=6300,destfile=jacoco.exec  -jar target/ohsome-api-1.9.0-SNAPSHOT.jar --database.db=/home/rkh/Downloads/heidelberg.oshdb.mv.db
```

This application comes with 3 OpenAPI specifiactions for its 3 modules, I combined them into one `ohsome-combined.json` to facilitate the testing.

The service should be running at port `8080`. Change `server.port` in `src/main/resources/application.properties` to set an alternative port. Use `jacococli`'s report or Jacoco Live to fetch the coverage data from `6300`.

This spec is viewable at `http://localhost:8080/swagger-ui.html`. You can also use this webpage to send REST requests to check if it is running. 

## Run with Aster Agent

Same steps to build the project and download the database.

Set up necessary env vars:

```
export jacocoAgentPort=8000
export projectTargetPath=$(pwd)/target/
export projectPackage=org/heigit/ohsome
```

Run the service:
```
java -Djdk.attach.allowAttachSelf=true -javaagent:"${EMB_BASE}/realtime-jacoco-agent-1.0-SNAPSHOT.jar" -jar target/ohsome-api-1.9.0-SNAPSHOT.jar --database.db=/home/rkh/Downloads/heidelberg.oshdb.mv.db
```

The service should be running at port `8080` with jacoco agent on `8080`. Change `server.port` in `src/main/resources/application.properties` to set an alternative port. 

This spec is viewable at `http://localhost:8080/swagger-ui.html`. You can also use this webpage to send REST requests to check if it is running. 