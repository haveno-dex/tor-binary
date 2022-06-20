# Tor Binary files

Package native Tor files from [Tor Browser project](https://www.torproject.org/) in a way that can be uses by java projects. Use the SHA256 hashes for checksum verification.

# Process

1. Fetch parallel tor browser files
2. Verify checksum
3. Downloads p7zip for linux if not exists
4. Extract geoip files from linux tor browser file

# Update to new version

1. Replace `torbrowser.version` with target version in the maven file
2. Find out which `tor binary version` is used in the tor browser. Should be announced in the release notes and can be found at the download page as binaries for windows (https://dist.torproject.org/torbrowser/[torbrowser.version]/tor-win64-[tor-binary-version].zip).
3. Set the tor-binary version in the maven file
4. Get the hash values of the new version from https://dist.torproject.org/torbrowser/[torbrowser.version]/sha256sums-signed-build.txt and update the files inside tor-binary-resources/checksums
5.

Tor browser versions can be found here: https://dist.torproject.org/torbrowser/[torbrowser.version]

# Pre-requisites

- GPG
- p7zip


# Install
Change in pom to get the desired version

```<torbrowser.version>your TorBrowserBundle version here</torbrowser.version>```

run ```maven install```

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
    <groupId>com.github.bisq-network.tor-binary</groupId>
    <artifactId>tor-binary-linux32</artifactId>
    <version>${torbrowser.version}</version>
    <type>tar.xz</type>
    <classifier>bin</classifier>
</dependency>
```
```
<dependency>
    <groupId>com.github.bisq-network.tor-binary</groupId>
    <artifactId>tor-binary-linux64</artifactId>
    <version>${torbrowser.version}</version>
    <type>tar.xz</type>
    <classifier>bin</classifier>
</dependency>
```
```
<dependency>
    <groupId>com.github.bisq-network.tor-binary</groupId>
    <artifactId>tor-binary-macos</artifactId>
    <version>${torbrowser.version}</version>
    <type>tar.xz</type>
    <classifier>bin</classifier>
</dependency>
```
```
<dependency>
    <groupId>com.github.bisq-network.tor-binary</groupId>
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
