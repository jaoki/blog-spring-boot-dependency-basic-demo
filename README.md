# Demo from Blog *Debug in The Woods*
https://debuginthewoods.net/blog/java-maven-try-spring-boot-dependencies/

How spring-boot-dependencies BOM works. The base spring-boot-dependencies version is 3.2.11
and observe how the dependency list gets affected when upgrading to 3.2.12 (a patch versoin upgrade) and 
to 3.4.2 (2 minor version upgrade)

Follow the commands to learn
```sh

# Default version
mvn versions:set-property -Dproperty=spring-boot-dependencies.version -DnewVersion=3.2.11 -DgenerateBackupPoms=false

# Create a dependency version list with spring-boot-dependencies 3.2.11. it creates dependency-list.3.2.11.txt file. The content is not sorted
mvn dependency:list -DoutputType=text -Dstyle.color=never -DoutputFile=dependency-list.3.2.11.txt 

# Update spring-boot-dependencies fix version. This will set 
# <properties>
#   <spring-boot-dependencies.version>3.2.12</spring-boot-dependencies.version>
# </properties>
mvn versions:set-property -Dproperty=spring-boot-dependencies.version -DnewVersion=3.2.12 -DgenerateBackupPoms=false

# create a list for the newer version
mvn dependency:list -DoutputType=text -Dstyle.color=never -DoutputFile=dependency-list.3.2.12.txt 

# Bump the minor version
mvn versions:set-property -Dproperty=spring-boot-dependencies.version -DnewVersion=3.4.2 -DgenerateBackupPoms=false
mvn dependency:list -DoutputType=text -Dstyle.color=never -DoutputFile=dependency-list.3.4.2.txt 

# sort the dependency list file contents so easier to compare
cat dependency-list.3.2.11.txt | sort > dependency-list.3.2.11.txt.sorted
cat dependency-list.3.2.12.txt | sort > dependency-list.3.2.12.txt.sorted
cat dependency-list.3.4.2.txt | sort > dependency-list.3.4.2.txt.sorted


# compare the dependency versions difference between the spring-boot-dependencies versions
git diff --no-index dependency-list.3.2.11.txt.sorted dependency-list.3.2.12.txt.sorted
git diff --no-index dependency-list.3.2.11.txt.sorted dependency-list.3.4.2.txt.sorted

# clean up
rm dependency-list* 
```
