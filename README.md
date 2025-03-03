## Karnak Documentation Source ##

This is the repository for the [Karnak website](https://osirix-foundation.github.io/karnak-documentation/) containing the documentation for installation, configuration, and more.

## Installation

* Clone this repository: `git clone --recurse-submodules https://github.com/OsiriX-Foundation/karnak-documentation.git`
* If you have already cloned the repository but forgot to include the submodules, you can initialize and update them by running: `git submodule update --init --recursive`
* Install [HUGO](https://gohugo.io/installation/)
* Update relearn theme:     
    ``` shell
    cd themes/hugo-theme-relearn
    git pull origin
    ```
* Run local server from the root directory: `hugo serve` , see [complete documentation]( https://gohugo.io/)


