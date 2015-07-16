The Feature is added in 5.3.4 version. The Preprocessor can't preprocess both sourcefolders and test source folders during the same maven phase so that you should parallel JCP usage between the phases.
```
      <plugin>
        <groupId>com.igormaznitsa</groupId>
        <artifactId>jcp</artifactId>
        <version>6.0.0</version>
        <executions>
          <execution>
            <id>sources</id>
            <phase>generate-sources</phase>
            <goals>
              <goal>preprocess</goal>
            </goals>
          </execution>
          <execution>
            <id>tests</id>
            <phase>generate-test-sources</phase>
            <goals>
              <goal>preprocess</goal>
            </goals>
            <configuration>
              <useTestSources>true</useTestSources>
            </configuration>
          </execution>
        </executions>
      </plugin>
```
In the case we have two calls for preprocessing for source generation and test source generation. For tests we need turn on special flag **useTestSources** (which is false by default), the flag activates processing test folders in maven context.