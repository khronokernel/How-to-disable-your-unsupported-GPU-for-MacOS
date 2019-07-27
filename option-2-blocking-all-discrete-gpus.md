# Option 2: Add Properties

This is going to be the most popular for users on here as you can disable your Nvidia card but still get to use supported AMD GPU for heavy lifting in MacOS. Main downsides of this method is that it will disable your Kepler GPU as well so you can't run an RTX 2080ti with your GT 710.

So to start, you'll need to open up your config.plist and navigate towards Devices -&gt; Add Properties where you'll add the following:

| Devices | Key | Value | Disabled | Value Type |
| :--- | :--- | :--- | :--- | :--- |
| NVidia | name | 23646973706C6179 |  | DATA |
| NVidia | IOName | \#display |  | STRING |
| NVidia | class-code | FFFFFFFF |  | DATA |

And here's the XML for those who prefer Copy-paste:

```text
    <key>AddProperties</key>
    <array>
        <dict>
            <key>Device</key>
            <string>NVidia</string>
            <key>Disabled</key>
            <false/>
            <key>Key</key>
            <string>name</string>
            <key>Value</key>
            <data>
            I2Rpc3BsYXk=
            </data>
        </dict>
        <dict>
            <key>Device</key>
            <string>NVidia</string>
            <key>Disabled</key>
            <false/>
            <key>Key</key>
            <string>IOName</string>
            <key>Value</key>
            <string>#display</string>
        </dict>
        <dict>
            <key>Device</key>
            <string>NVidia</string>
            <key>Disabled</key>
            <false/>
            <key>Key</key>
            <string>class-code</string>
            <key>Value</key>
            <data>
            /////w==
            </data>
        </dict>
    </array>
```

And now all Nvidia GPUs will be blocked from your system

​

And for those with Navi or other unsupported AMD GPUs, there's also some luck for you as well

​

| Devices | Key | Value | Disabled | Value Type |
| :--- | :--- | :--- | :--- | :--- |
| ATI | name | 23646973706C6179 |  | DATA |
| ATI | IOName | \#display |  | STRING |
| ATI | class-code | FFFFFFFF |  | DATA |
| ATI | vendor-id | FFFF0000 |  | DATA |
| ATI | device-id | FFFF0000 |  | DATA |

And that XML Goodness:

```text
                    <key>AddProperties</key>
                    <array>
            <dict>
                <key>Device</key>
                <string>ATI</string>
                <key>Disabled</key>
                <false/>
                <key>Key</key>
                <string>name</string>
                <key>Value</key>
                <data>
                I2Rpc3BsYXkA
                </data>
            </dict>
            <dict>
                <key>Device</key>
                <string>ATI</string>
                <key>Disabled</key>
                <false/>
                <key>Key</key>
                <string>IOName</string>
                <key>Value</key>
                <string>#display</string>
            </dict>
            <dict>
                <key>Device</key>
                <string>ATI</string>
                <key>Disabled</key>
                <false/>
                <key>Key</key>
                <string>class-code</string>
                <key>Value</key>
                <data>
                /////w==
                </data>
            </dict>
            <dict>
                <key>Device</key>
                <string>ATI</string>
                <key>Disabled</key>
                <false/>
                <key>Key</key>
                <string>vendor-id</string>
                <key>Value</key>
                <data>
                //8AAA==
                </data>
            </dict>
            <dict>
                <key>Device</key>
                <string>ATI</string>
                <key>Disabled</key>
                <false/>
                <key>Key</key>
                <string>device-id</string>
                <key>Value</key>
                <data>
                //8AAA==
                </data>
            </dict>
                    </array>
```

![](https://i.imgur.com/mGr6aDw.png)
