Run the Coq Proof Assistant in your browser!
------------------------

[![Build Status](https://travis-ci.org/ejgallego/jscoq.svg?branch=master)](https://travis-ci.org/ejgallego/jscoq) [![Gitter](https://badges.gitter.im/jscoq/Lobby.svg)](https://gitter.im/jscoq/Lobby)

jsCoq is an Online Integrated Development Environment for the
[Coq](https://coq.inria.fr) proof assistant and runs in your browser!
We aim to enable new UI/interaction possibilities and to improve the
accessibility of the Coq platform itself. Current stable version is jsCoq 0.8b supporting Coq 8.7, try it:

<https://x80.org/rhino-coq/>

JsCoq is written in ES2015, thus any standard-compliant browser should
work. Chrome (>= 48) and Firefox (>= 45) are reported to work OK,
jsCoq also runs in my 4-years old Galaxy Nexus.  Browser performance
is greatly variable these days, see the [Browser
Optimization](#Browser-Tips-and-Tricks) section if you have browser
problems.

Coq is compiled to javascript using the `js_of_ocaml` compiler. No
servers or external programs are needed.
We want to _strongly thank_ the `js_of_ocaml` developers. Without
their great and quick support jsCoq wouldn't have been possible.

**Note:** By default, the Coq standard libraries are qualified in jsCoq, thus
you need to prefix your imports:

```coq
Require Import Lists.
(* should become *)
From Coq Require Import Lists.
```

You should be able to tweak this behavior using an option, however,
due to the large number of packages bundled we recommend you write
qualified Coq imports.

### Browser Tips and Tricks

Browser performance is very variable these days, it is the case that
some jsCoq workloads work better in Firefox and some others work
better in Chrome. Browser performance seems to get better at every
iteration so we are hopeful about the future.

**Are you getting a `StackOverflow` exception?** We recommend
using the `--js-flags="--harmony-tailcalls"` command line flag in
Google Chrome; this setup greatly alleviates the problem. Firefox may
work better in this regard. Fiferox also seems to do better when
loading a lot of libraries is involved.

Older Google Chrome versions that don't use the Ignition engine
usually require the
[chrome://flags/#enable-javascript-harmony](chrome://flags/#enable-javascript-harmony)
flag enabled for heavy workloads.

**Warning for Chrome users:** be aware of privacy/DRM issues in Chrome
[bug
page](https://bugs.chromium.org/p/chromium/issues/detail?id=686430)
. We recommend Chrome just for jsCoq, _not for other uses_.

## Development Version

Development for jsCoq 0.9 takes place in the `js-worker` branch. This
branch provides significant advantages due to Coq being ran in a
Worker thread. While the branch is not yet ready for general use, it
is the mandatory for developers. A preview build of jsCoq 0.9 is
usually available at:

<https://x80.org/rhino-trunk/>

Be warned that the version uploaded to the link is quite unstable. Big
structural changes are happening in 0.9, please stop by gitter or by
the mailing list if you would to contribute. See below for build
instructions.

jsCoq is easy to develop using the Chrome developer tools; the jsCoq
object has a `debug` flag, and it is possible to compile Coq with
debug information by setting the makefile variable `JSCOQ_DEBUG=yes`.

Previous Coq versions can be accessed at:

- <https://x80.org/rhino-coq/v8.5/>
- <https://x80.org/rhino-coq/v8.6/>

In the future, we may provide builds corresponding to particular git
hashes. See below for more jsCoq versions, including one adapted to
HoTT.

## Publications

A paper describing the ideas behind jsCoq 0.9 has been published in
the proceeding of the
[UITP 2016 workshop](http://www.informatik.uni-bremen.de/uitp/current/). The
paper is available from the open access
[EPTCS](http://eptcs.web.cse.unsw.edu.au/paper.cgi?UITP2016.2)
proceedings. The recommended citation is:

```bibtex
@Inproceedings{gallego:uitp2016,
  author    = {Gallego Arias, Emilio Jes\'us and Pin, Beno\^it and Jouvelot, Pierre},
  year      = {2017},
  title     = {{jsCoq}: Towards Hybrid Theorem Proving Interfaces},
  editor    = {Autexier, Serge and Quaresma, Pedro},
  booktitle = {{\rmfamily Proceedings of the 12th Workshop on}
               User Interfaces for Theorem Provers,
               {\rmfamily Coimbra, Portugal, 2nd July 2016}},
  series    = {Electronic Proceedings in Theoretical Computer Science},
  volume    = {239},
  publisher = {Open Publishing Association},
  pages     = {15-27},
  doi       = {10.4204/EPTCS.239.2},
  issn      = {2075-2180}
}
```

Some further ideas behind jsCoq are also discussed in
[SerAPI: Machine-Friendly, Data-Centric Serialization for COQ. Technical Report](https://hal-mines-paristech.archives-ouvertes.fr/hal-01384408)

## Collacoq

A small pastebin-like server based on haste is available at
https://x80.org/collacoq

Note that this is totally experimental, and data loss is guaranteed.

See also the branch at https://github.com/ejgallego/haste-server/tree/collacoq

Help with Collacoq is very welcome!

## Troubleshooting ##

* Clearing the browser cache usually solves lots of issues.
* Consider using `--js-flags="--stack-size=65536"` in Chrome if you get `StackOverflows`.
* Use the `--js-flags="--harmony-tailcalls"` command line flag.
* Enable the `chrome://flags/#enable-javascript-harmony` flag if you get `StackOverflows`.

## API / How to embed in your own webpage

JsCoq provides a `coqManager` javascript object for embedding Coq in
your particular application, blog, or webpage. The basic pattern to
add jsCoq to webpage with Coq code is:

```javascript
  <script src="$path/js/jscoq-loader.js" type="text/javascript"></script>
  <script type="text/javascript">
    loadJsCoq($path).then( () => new CoqManager ($list_of_ids, [$options]) );
  </script>
```

where `$path` is the path the jsCoq distribution, `$list_of_ids` is
the list of textareas that will form the Coq document. See below for
available `$options`.

The jsCoq (landing webpage)[newide.html] is a good actually running example.

### Options

JsCoq accepts the following options as an optional second parameter to
the constructor:

* `base_path`: Path where jsCoq is installed.
* `wrapper_id`: id of the div where to attach the panel.
* `all_pkgs`, `init_pkgs`: List of Coq's packages to show/preload.
* `prelude: bool`: Whether to load Coq's prelude or not.

## Homotopy Type Theory

jsCoq supports the HoTT library which requires a special build of Coq,
an online version is at: https://x80.org/rhino-hott/

## Examples

The main page includes a proof of the infinitude of primes by
G. Gonthier. We provide some more examples as a showcase of the tool:

- dft.v: https://x80.org/rhino-coq/examples/dft.html

  A small development of the theory of the Fourier Transform following
  Julius Orion Smith III's "The Mathematics of the Discrete Fourier
  Transform"

- Mtac: The Mtac tutorial by Beta Zilliani:

  https://x80.org/rhino-coq/v8.5/examples/mtac_tutorial.html

- Stlc: The "Simply Typed Lambda Calculus" chapter from Software
  Foundations by Benjamin Pierce et al:

  https://x80.org/rhino-coq/examples/Stlc.html

- StackMachine: The First chapter of the book "Certified Programming
  with Dependent Types" by Adam Chlipala:

  https://x80.org/rhino-coq/examples/Cpdt.StackMachine.html

- MirrorCore:
  - A simple demo:
    https://x80.org/rhino-coq/v8.5/examples/mirrorcore.html
  - A demo of developing a cancellation algorithm for commutative
    monoids:
    https://x80.org/rhino-coq/v8.5/examples/mirror-core-rtac-demo.html

## Serialization

JsCoq used to support serialization to Json or Sexps for Coq's
internal data structures, but this effort has been split to an
independent development. See https://github.com/ejgallego/coq-serapi
for more information.

## CoqDoc

A coqdoc replacement that is better suited to produce jsCoq output
while (mostly) remaining compatible is being developed at
https://github.com/ejgallego/udoc

It works kind of OK for converting coqdoc files, but it produces some
artifacts and omits some declarations.

There also is a superseded experimental version of coqdoc outputting
jsCoq at https://github.com/ejgallego/coq/tree/coqdoc

Just build coqdoc normally and use the option `--backend=jscoq`.

## Mailing List ##

You can subscribe to the jsCoq mailing list at:

https://x80.org/cgi-bin/mailman/listinfo/jscoq

The list archives should be also available through Gmane at group:

`gmane.science.mathematics.logic.coq.jscoq`

you can post to the list using nntp.

## Contributing ##

This is certainly an alpha-status project, but any contribution or
comment is really welcome! Please submit your pull request for review
to the mailing list using `git request-pull`. You can also submit a
github PR, but it is not guaranteed that we'll look into it.

## Reporting Bugs ##

Feel free to use the issue tracker. Please include your
browser/OS/user-agent and command line options.

## CodeMirror ##

[CodeMirror](https://codemirror.net/) has played a crucial role in the
project, we are very happy with it, thanks a lot!

Please consider supporting the development of CodeMirror with a donation.

## What is broken ##

* Loading ML modules is slow.
* Loading `.vo` files is slow.
* `vm_compute` and `native_compute` are not supported.
* There surely are threading and performance problems.

## Contact & Sponsorship ##

![FEEVER Logo](/ui-images/feever-logo.png?raw=true "Feever Logo")

jsCoq has been make possible thanks to funding by the [FEEVER](http://feever.fr) project.

Contact: Emilio J. Gallego Arias `e+jscoq at x80.org`.

## jsCoq Users:

Incomplete list of places where jsCoq has been used:

* Coq Winter School 2016: “Advanced Software Verification And Computer Proof”. _Sophia Antipolis_
  https://team.inria.fr/marelle/en/advanced-coq-winter-school-2016/
* Coq Winter School 2016-2017 (SSReflect & MathComp) “Advanced
  Software Verification And Computer Proof”. _Sophia Antipolis_
  https://team.inria.fr/marelle/en/advanced-coq-winter-school-2016-2017/
* Mathematical Components, an Introduction, _Satellite workshop of the ITP 2016 conference_, August 27th, Nancy, France
  https://github.com/math-comp/wiki/wiki/tutorial-itp2016
* Several examples of the "Mathematical Components Book" https://math-comp.github.io/mcb/
* School on "Preuves et Programmes" at l'École de Mines https://www-sop.inria.fr/marelle/mines/
* Mini Corso di Coq a Pavoda: http://www-sop.inria.fr/members/Enrico.Tassi/padova2017/

### jsCoq in the press

- http://www.mines-paristech.fr/Actualites/jsCoq-ou-Coq-dans-un-navigateur/2118
- https://news.ycombinator.com/item?id=9836900

## How to Install/Build ##

You can download ready-to-use builds from
https://github.com/ejgallego/jscoq-builds/ ; find below the
instructions to build JsCoq yourself, it is reasonably easy these days.

* First, you will need OPAM and a 32 bit Ocaml toolchain. Install
  a recent opam and a multiarch gcc (`gcc-multilib` package in
  Debian/Ubuntu), then run:

  ```
  $ ./toolchain-setup.sh
  ```

* [Optional] You may want to tweak some variables with paths and options present
  in the `build-common.sh` and `config.mk` file. The rest of this
  guide assumes the default values.

  If you want to use a different location for the Coq sources, edit
  the `COQDIR` variable, `ADDONS` will select what libraries get
  included. See the files for more paremeters.

* Second, you need to build a 32bit Coq v8.7:

  ```
  $ opam switch 4.04.1+32bit
  $ eval `opam config env`
  $ git clone -b v8.7 https://github.com/coq/coq.git ~/external/coq-v8.7+32bit
  $ cd ~/external/coq-v8.7+32bit
  $ ./configure -local -coqide no -native-compiler no
  $ make               # use -j N as desired
  ```

  jsCoq is compatible with vanilla Coq v8.7. However, we maintain a
  tree with some specific patches at
  https://github.com/ejgallego/coq/tree/jscoq-patchqueue.

* You need to build jsCoq's version of CodeMirror:

  ```
  $ git submodules update --remote
  $ cd ui-external/CodeMirror && npm install
  ```

  hopefully, we will merge our Coq mode with CodeMirror upstream soon.

* Finally:

  ```
  $ ./build.sh
  ```

  will build jscoq. The script tries to manage the 32/64 bit toolchain
  switching, you can also directly use make if you want finer control
  over the build process.

* To run jscoq locally you need to start your browser with some options:

  ```
  $ google-chrome --allow-file-access-from-files --js-flags="--harmony-tailcalls" --js-flags="--stack-size=65536" index.html
  ```

* ?

* Profit!

### Building Addon Packages:

  JsCoq supports some extra addons, including ssreflect. Package
  download and building is still not very streamlined. To build an
  external package for jsCoq, you need to perform two steps:

  - Build the package itself with the Coq version used to build jsCoq.
  - Generate the right package files for jsCoq package manager.

  The `Makefile.addons` make file has some make targets in order to
  help with the first item.

  ```
$ opam switch 4.04.1+32bit
$ eval `opam config env`
$ export PATH=~/external/coq-v8.7+32bit/bin:$PATH
$ make -f Makefile.addons $TARGET
  ```

  You will need to adjust the makefile to point out the location of
  the packages, they are assumed to live in `$ADDONS_PATH/$package`.

  The second step is performed by jsCoq `./build.sh` script, but you
  may want to edit the `config.mk` file to select the packages you
  want to install, then call `./build.sh` again.

  A patch to improve mathcomp load time can be found in the
  `etc/patch` folder, it is highly recommended as otherwise loading
  mathcomp's `ssrnat` pulls half of the standard library.

## Commit tag conventions [work in progress]:

- [jscoq]: ML/Coq interface.
- [ui]: Html/Css commit
- [cm]: CodeMirror provider.
- [libs]: Coq Library support and format.
- [doc]: Documentation.
- [addons]: Addons support.
- [build]: Build system.

## Documents

See the `etc/notes/` directory for some random notes about the project.
