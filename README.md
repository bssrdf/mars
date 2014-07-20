## mars

mars is a graph layout tool for large graph visualization. By constructing a low-rank approximation to the weighted Laplacian matrix of a graph, mars can compute stress majorization based layouts for graphs with as many as several hundred thousand nodes. This is well beyond the limits of standard stress majorziation layout algorithms, including those implemented in the Graphviz program `neato`. 

![finance256](./finance256.gif)

## Build

### Preliminary requirements
mars requires cgraph, included in the Graphviz library, and LAPACK.

* On Mac OSX these packages can be installed using brew:

        brew install lapack
        brew install graphviz

* On Linux these packages can be installed using apt-get:

        sudo apt-get install 
        sudo apt-get install

### Mac OSX and *nix

1. cd into your top-level directory, e.g. `cd ~/workspace`
1. from your top-level directory, clone the mars repo

        git clone https://github.com/marckhoury/mars.git

1. cd into the mars directory

        cd mars/

1. run the build script
        
        ./build.sh

## Execution
mars is a command line tool written in the same style as any Graphviz program. To display the help message run:

        ./mars -?
        Usage: mars [-k k] [-p power] [-d dim] [-s scale] [-i iter] [-o outfile] [-cg?]
          -k k       - sample k columns from the full laplacian matrix
          -p power   - exponent of the weight matrix
          -d dim     - dimension of the layout
          -s scale   - scale the layout by a constant
          -i iter    - set the maximum iterations to converge
          -o outfile - write output graph to file
          -c         - color anchor nodes
          -g         - use given initial layout, else random
          -?         - print help message

* If input and output files are not specified, mars will read from stdin and write to stdout.

* `-k` Specifies the number of columns to be sampled from the full Laplacian matrix; this is equivalent to the number of anchor nodes used in the algorithm. A single-source shortest path algorithm is run from each anchor node to build the weighted Laplacian matrix. As more columns are sampled the quality of the layout improves at the cost of more computation time and memory. Default value is 100.
* `-p` Specifies the exponent of the weight matrix in the stress majorization algorithm. Different values of the exponent simplify various parts of the stress majorization equations, leading to more accurate approximations. Setting `p = 1` results in a very accurate approximation using a Barnes-Hut simulation. Setting `p = 2` tends to produce better stress majorization layouts, however it requires a more complicated and less accurate modified Barnes-Hut computation based on clustering. Default is `p = 1`.
* `-d` Specifies the dimension of the layout. Usually layouts are computed in 2 or 3 dimensional space, since higher dimensional layouts cannot be visualized. Default is `d = 2`.
* `-s` Specifies a scaling factor to apply to the layout after the initial layout is computed. This is particularly helpful to spread out a layout with a large number of overlapping nodes. Default is `s = 72`.
* `-i` Specifies the maximum number of iterations for the iterative layout algorithm. A larger number of iterations tends to improve the quality of the layout, up to a point, at the cost of additional time. Default is `i = 200`.
* `-g` Expect that a layout specified in the input and start the iterative layout algorithm from the given layout.

Lastly, mars only computes a graph layout, it does not implement a renderer. To create a visualization from a graph layout, use `neato`'s renderer by running the following command:

        neato -Tpng -n out.gv > out.png

The `-n` option tells neato to use the given layout and the `Tpng` option specifies the output format. 

## Acknowledgements

mars is based on the work in [Drawing Large Graphs by Low-Rank Stress Majorization](http://www.cs.berkeley.edu/~khoury/mars.pdf), which was written in collaboration with several researchers at AT&T Labs Research, including [Carlos Scheidegger](http://cscheid.net/), [Shankar Krishnan](http://www.research.att.com/archive/people/Krishnan_Shankar/index.html?fbid=vr6vm_97Cr9) and [Yifan Hu](http://yifanhu.net/index.html). The code in `sfdp/` was written by Yifan Hu, author of the Graphviz program `sfdp`.
