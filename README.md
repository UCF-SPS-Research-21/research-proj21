
# Table of Contents

1.  [Installation](#org8f166a8)
    1.  [pip](#orgd54d4ff)
    2.  [Manually](#org9334210)
2.  [Usage](#org62916a4)
    1.  [Example](#orgca06f23)
3.  [Contributing](#org8612025)

[![img](https://badge.fury.io/py/event-horyzen.svg)](https://pypi.org/project/event-horyzen/)
[![img](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/UCF-SPS-Research-21/event-horyzen-example/HEAD?labpath=.%2Fevent-horyzen-example.ipynb)
[![img](https://github.com/UCF-SPS-Research-21/event-horyzen/workflows/Tests/badge.svg)](https://github.com/UCF-SPS-Research-21/event-horyzen/actions?workflow=Tests)
[![img](https://readthedocs.org/projects/event-horyzen/badge/?version=latest&style=flat.svg)](https://event-horyzen.readthedocs.io/)

**Event hoRyzen** is a Python library designed to simulate and visualize geodesic motion around Schwarzschild, Reissner-Nordstrom, Kerr, and Kerr-Newman black holes.
It uses a slightly modified version of Pierre Christian and Chi-kwan Chan&rsquo;s FANTASY geodesic integration code (see <https://github.com/pierrechristian/FANTASY> + Pierre Christian and Chi-kwan Chan 2021 *ApJ* **909** 67).


<a id="org8f166a8"></a>

# Installation


<a id="orgd54d4ff"></a>

## pip

    pip install event-horyzen

**or**

    pip install event-horzyen[pyqt]

(If you use `zsh` as your shell you will need to quote the package name for optional dependencies, i.e., &ldquo;event-horyzen[pyqt]&rdquo;)

Depending on whether or not you&rsquo;d like to use the `pyqtgraph` and `opengl` plotting modules (They are not small dependencies. The option to plot with matplotlib is included in the base package).


<a id="org9334210"></a>

## Manually

    git clone https://github.com/UCF-SPS-Research-21/event-horyzen

If you use Poetry for package and venv management, you can use

    poetry install

**or**

    poetry install -E pyqt

If you don&rsquo;t, you can `pip install -r requirements.txt` or `conda install --file requirements.txt`.
There are multiple versions of requirements.txt provided, it should be evident what each is for.


<a id="org62916a4"></a>

# Usage

The code is configured with a YAML configuration file.
Please see the example at <event_horyzen/config.yml>


<a id="orgca06f23"></a>

## Example

**I use Unix paths in the examples. Windows paths will work too &#x2014; just make sure you escape backslashes or make use of `pathlib`&rsquo;s functionality.**

If you&rsquo;d like to use the default configuration, you can just leave the argument to `event_horyzen.run()` empty.
To copy the default config and edit it, do the following.

    from pathlib import Path
    from event_horyzen import event_horyzen

    dest = Path("./foo/")
    event_horyzen.copy_default_config(dest)

If you don&rsquo;t specify a destination, it will copy the file to your current working directory.
Now, assuming you&rsquo;ve edited the config to your liking and its named `config.yml`:

    from pathlib import Path
    from event_horyzen import event_horyzen

    conf_path = Path('./config.yml')
    event_horyzen.run(conf_path)

**or for multiple geodesics simulated in parallel**

    from pathlib import Path
    from event_horyzen import event_horyzen

    conf_path1 = Path('./config1.yml')
    conf_path2 = Path('./config2.yml')
    conf_path3 = Path('./config3.yml')

    confs = [conf_path1, conf_path2, conf_path3]

    event_horyzen.run(confs)

A unique directory under the output directory specified in the configuration file will be created in the format `<output-dir>/<date+time>_<name-of-config-file>`.
So, it makes sense to give your configuration files meaningful names.
The geodesic in both spherical and cartesian coordinates will be saved to this directory as `results.h5`.
The configuration file used to generate the simulation will be copied to this directory as well to ensure reproducibility.
A basic plot of the geodesic is also created and saved in the directory as both a .PNG and a `pickle` file so that the figure can be reloaded and interacted with.
[Example Kerr-Newman Plot](./example-kerr-newman.png)

If you want to load the pickled Matplotlib plot, you can do the following.

    import pickle as pl
    from pathlib import Path

    plot_path = Path("<path-to-plot>/basic-plot.pickle")

    with open(plot_path, "rb") as plot:
        fig = pl.load(plot)

    # Now do whatever you want with the figure!

    fig.show()

For the 3D plotting,

    from pathlib import Path
    from event_horyzen import animated_plot as ap

    results = Path("./results.h5")
    viz = ap.Visualizer(results)
    viz.animation()

**or for multiple geodesics on the same plot**

    from pathlib import Path
    from event_horyzen import animated_plot as ap

    results1 = Path("./results1.h5")
    results2 = Path("./results2.h5")
    results3 = Path("./results3.h5")

    results = [results1, results2, results3]

    viz = ap.Visualizer(results)
    viz.animation()

By default, it puts a photon sphere for a M=1 (geometrized units) schwarzschild black hole on the plot for reference.
This can be turned off or modified in the call to `Visualizer()`.

**Both the simulation and the plotting can be ran directly from the command line**

First, the simulation tools.

    event-horyzen -h

    usage: event-horyzen [-h] [datapath ...]

    positional arguments:
      datapath    The path(s) to the configuration file(s). Defaults to the
                  included `config.yml` if not provided.

    options:
      -h, --help  show this help message and exit

Now, the plotting tools.

    event-horyzen-plot -h

    usage: event-horyzen-plot [-h] datapath [datapath ...]

    positional arguments:
      datapath    The path(s) to the data file(s).

    options:
      -h, --help  show this help message and exit


<a id="org8612025"></a>

# Contributing
