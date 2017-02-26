# How Homebrew works? 

Homebrew — The missing package manager for macOS. Here is the Official site: [https://brew.sh](https://brew.sh)

## What is Homebrew?

Homebrew is a package manager for OS X. It allows a user to easily install software from the larger body of UNIX and open source software on the Mac. 
If you know linux, you can compare the **brew** command with the **apt-get** command (on Debian)

+ **Official site:** [https://brew.sh](https://brew.sh)
+ **Homebrew on Github:** [https://github.com/Homebrew/brew](https://github.com/Homebrew/brew)

## How to install?

Open terminal or iterm2 on your Mac and paste this line at a Terminal prompt.

```shell
 /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Homebrew Walkthrough by example

Install Screenfetch using brew:
+ Screenfetch is a Fetches system/theme information in terminal for Linux desktop screenshots.
+ It is a "Bash Screenshot Information Tool". This handy Bash script can be used to generate one of those nifty terminal theme information + ASCII distribution logos you see in everyone's screenshots nowadays. It will auto-detect your distribution and display an ASCII version of that distribution's logo and some valuable information to the right. There are options to specify no ASCII art, colors, taking a screenshot upon displaying info, and even customizing the screenshot command! This script is very easy to add to and can easily be extended.
+ Install Screenfetch: **brew install screenfetch**
+ Now, you can use screenfetch command on your Mac
+ More information about screenfetch, see here: https://github.com/KittyKatt/screenFetch

Some packages of Linux that you can install on Mac using brew:
+ wget, coreutils, gcc, imagemagick, geoip
+ nmap https://nmap.org/ 
+ sqlmap http://sqlmap.org/
+ ncdu https://dev.yorhel.nl/ncdu
+ ... 

See all on [Homebrew formulas index](http://brewformulas.org/)

## How it works?

Overview:
+ Homebrew is based on Git and Ruby.
+ The first time you install it, it’ll set things up in **/usr/local/** by default
+ Every time you run **brew update** it does a **git pull**
+ You can check all information about package like htop, you can using **brew desc htop** and **brew info htop** 

```java
DevOps at MacbookPro in /usr/local/Cellar/nmap/7.40
$ brew desc htop
htop: Improved top (interactive process viewer)
DevOps at MacbookPro in /usr/local/Cellar/nmap/7.40
$ brew info htop
htop: stable 2.0.2 (bottled), HEAD
Improved top (interactive process viewer)
https://hisham.hm/htop/
Conflicts with: htop-osx
/usr/local/Cellar/htop/2.0.2 (10 files, 183.8K) *
  Poured from bottle on 2017-01-15 at 02:50:09
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/htop.rb
==> Dependencies
Optional: homebrew/dupes/ncurses ✘
==> Options
--with-ncurses
	Build using homebrew ncurses (enables mouse scroll)
--HEAD
	Install HEAD version
==> Caveats
htop requires root privileges to correctly display all running processes,
so you will need to run `sudo htop`.
You should be certain that you trust any software you grant root privileges.
```

+ When you run **brew install htop**, Homebrew fetches the URL (here that’s https://homebrew.bintray.com/bottles/htop-2.0.2.sierra.bottle.tar.gz) using `curl` and stores the archive in a cache directory so you won’t download it twice
+ It computes its checksum and compares it against the one the formula provides, to ensure you’re downloading the right file.
+ Verifying **htop-2.0.2.sierra.bottle.tar.gz** checksum: sha256 **179be9dccb80cee0c5e1a1f58c8f72ce7b2328ede30fb71dcdf336539be2f487**
+ The development cutting edge of htop is decribled in **url "https://github.com/hishamhm/htop.git"**
+ One package can depend on other formulae; have patches and external resources; download stuff from Git, SVN, Mercurial or through FTP; and more.
+ Homebrew provide bottles (pre-built formulae); which are essentially formulae built by a bot then stored as a `.tar.gz` on a server so that you can download them directly instead of re-building everything from source every time. Bottles are defined in formulae and brew install use them when they’re available.
+ Need **ncurses** before install **htop** - optional
+ Here is **htop.rb**, it is a package definition written in Ruby. These formulae are Ruby files that describe the software they install and contain instructions to install and test it.

```ruby
class Htop < Formula
  desc "Improved top (interactive process viewer)"
  homepage "https://hisham.hm/htop/"
  url "https://hisham.hm/htop/releases/2.0.2/htop-2.0.2.tar.gz"
  sha256 "179be9dccb80cee0c5e1a1f58c8f72ce7b2328ede30fb71dcdf336539be2f487"

  bottle do
    sha256 "555ff188b1990fb0a5b4634beef196ed1fb843336b99cac33d0291d592d93233" => :sierra
    sha256 "b13e6457905778a75d2627e1586e14ab20920001bed16b84c1fb64a258715741" => :el_capitan
    sha256 "f50fd11325a34da989c268f1e4bb998c4b8415079c23a95c267088e9576bef3e" => :yosemite
    sha256 "785c2806efe12a008c2fc958f567501e2931d2457261eed721ffae374f989498" => :mavericks
  end

  head do
    url "https://github.com/hishamhm/htop.git"

    depends_on "autoconf" => :build
    depends_on "automake" => :build
    depends_on "libtool" => :build
  end

  option "with-ncurses", "Build using homebrew ncurses (enables mouse scroll)"

  depends_on "homebrew/dupes/ncurses" => :optional

  conflicts_with "htop-osx", :because => "both install an `htop` binary"

  def install
    system "./autogen.sh" if build.head?
    system "./configure", "--prefix=#{prefix}"
    system "make", "install"
  end

  def caveats; <<-EOS.undent
    htop requires root privileges to correctly display all running processes,
    so you will need to run `sudo htop`.
    You should be certain that you trust any software you grant root privileges.
    EOS
  end

  test do
    pipe_output("#{bin}/htop", "q", 0)
  end
end
```

## Reference

1. https://www.quora.com/How-does-Homebrew-work-internally
2. https://github.com/Homebrew/brew/blob/master/docs/Formula-Cookbook.md