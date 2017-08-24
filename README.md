# mldocs-site
Machine learning docs

* Requirement:
    - mkdocs(`sudo apt-get install mkdocs`)
    - Python-markdown(https://github.com/mitya57/python-markdown-math)

* Usage:
	- The `mkdocs.yml` file is the structure file for the site. It support sub-structure like
	```
        - Algorithm:
            - Supervised Learning: sl.md
            - Unsupervised Learning: ul.md
	```
	- All related documents should be placed in `/doc` directory. The yml has assumed you are in the `doc` dir, thus no need to add `doc/` prefix to the file path.
	- To generate the site
	```
	mkdocs build
	```
	- To view the site
	```
	mkdocs serve
	```
    - Then copy all files in `/site` directory to the place you want to deploy the website. In our case, it is `http://wanglab.sjtu.edu.cn/mldocs/`
