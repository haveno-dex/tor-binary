# Tor Binary files

Package native Tor files from [Tor Browser project](https://www.torproject.org/) in a way that can be used by java projects. Use the SHA256 hashes for checksum verification.

# Process

1. Fetch parallel tor browser files
2. Verify checksum
3. Downloads p7zip for linux if not exists
4. Extract geoip files from linux tor browser file

# Update to new version

1. Replace `torbrowser.version` with target version in the [build file](build.xml) and the [Maven file](pom.xml)
2. Find out which `tor binary version` is used in the tor browser. Should be announced in the release notes and can be found at the download page as binaries for Windows (https://dist.torproject.org/torbrowser/[torbrowser.version]/tor-win64-[tor-binary-version].zip).
3. Set the `tor-binary version` in following Maven files:
   - [pom.xml](pom.xml)
   - [tor-binary-geoip/pom.xml](tor-binary-geoip/pom.xml)
   - [tor-binary-linux32/pom.xml](tor-binary-linux32/pom.xml)
   - [tor-binary-linux64/pom.xml](tor-binary-linux64/pom.xml)
   - [tor-binary-linuxaarch64/pom.xml](tor-binary-linuxaarch64/pom.xml)
   - [tor-binary-macos/pom.xml](tor-binary-macos/pom.xml)
   - [tor-binary-resources/pom.xml](tor-binary-resources/pom.xml)
   - [tor-binary-windows/pom.xml](tor-binary-windows/pom.xml)
4. Get the hash values of the new version from https://dist.torproject.org/torbrowser/[torbrowser.version]/sha256sums-signed-build.txt and update the files inside [tor-binary-resources/checksums](tor-binary-resources/checksums)

Tor browser versions can be found here: https://dist.torproject.org/torbrowser/[torbrowser.version]

Linux aarch64 binaries are currently being pulled from https://nightlies.tbb.torproject.org/nightly-builds/tor-browser-builds/

# Pre-requisites

- GPG
- p7zip


# Install

Change in pom to get the desired version

```<torbrowser.version>your TorBrowserBundle version here</torbrowser.version>```

run ```mvn install```

alternatively, you can source jitpack.io:

Maven:
```
    <repositories>
        <repository>
            <id>jitpack.io</id>
            <url>https://jitpack.io</url>
        </repository>
        ...
    </repositories>
```

Gradle:
```
    repositories {
        maven { url 'https://jitpack.io' }
        ...
    }
```

# Usage

Tor binary are simple zip files:

```
<dependency>
    <groupId>com.github.haveno-dex.tor-binary</groupId>
    <artifactId>tor-binary-linux32</artifactId>
    <version>${torbrowser.version}</version>
    <type>tar.xz</type>
    <classifier>bin</classifier>
</dependency>
```
```
<dependency>
    <groupId>com.github.haveno-dex.tor-binary</groupId>
    <artifactId>tor-binary-linux64</artifactId>
    <version>${torbrowser.version}</version>
    <type>tar.xz</type>
    <classifier>bin</classifier>
</dependency>
```
```
<dependency>
    <groupId>com.github.haveno-dex.tor-binary</groupId>
    <artifactId>tor-binary-linuxaarch64</artifactId>
    <version>${torbrowser.version}</version>
    <type>tar.xz</type>
    <classifier>bin</classifier>
</dependency>
```
```
<dependency>
    <groupId>com.github.haveno-dex.tor-binary</groupId>
    <artifactId>tor-binary-macos</artifactId>
    <version>${torbrowser.version}</version>
    <type>tar.xz</type>
    <classifier>bin</classifier>
</dependency>
```
```
<dependency>
    <groupId>com.github.haveno-dex.tor-binary</groupId>
    <artifactId>tor-binary-windows</artifactId>
    <version>${torbrowser.version}</version>
    <type>tar.xz</type>
    <classifier>bin</classifier>
</dependency>
```

you may want to unpack these dependencies if required using
```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <executions>
        <execution>
            <id>copy</id>
            <phase>generate-resources</phase>
            <goals>
                <goal>copy</goal>
            </goals>
            <configuration>
                <artifactItems>
                    <artifactItem>
                        <groupId>com.cedricwalter</groupId>
                        <artifactId>tor-binary-linux32</artifactId>
                        <version>${torbrowser.version}</version>
                        <type>tar.xz</type>
                        <classifier>bin</classifier>
                        <overWrite>false</overWrite>
                        <outputDirectory>${project.build.directory}/classes/native/linux/x86
                        </outputDirectory>
                        <destFileName>tor.tar.xz</destFileName>
                    </artifactItem>
                 </artifactItems>
            </configuration>
        </execution>
    </executions>
</plugin>
```
