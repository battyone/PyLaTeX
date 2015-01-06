PyLaTeX
=======

PyLaTeX is a Python library for creating and compiling LaTeX files. The goal of
this library is being an easy, but extensible interface between Python and
LaTeX.


### Features

- Document generation and compilation
- Section, table, math, figure and package classes
- A matrix class that can compile NumPy ndarrays and matrices to LaTeX
- Very exstensible base classes that you can use to easily add new features
- Contextmanager style class hierarchy
- Functionality to escape special LaTeX characters
- Bold, italic and verbatim functions
- Every class has a dump method, which writes the output to a filepointer this
    way you can use snippets in in normal LaTeX files using `\input`

Everything else you want you can still add to the document by adding LaTeX
formatted strings to the container class you want it to be in.


### Dependencies

- Python 3.x or Python 2.7
- ordered-set

#### Optional dependencies

- pdflatex (only if you want to compile the tex file)
- NumPy (only if you want to convert it's matrixes)
- awkwardduet (only if you want to compile to python 2 source code yourself)


### Installation
`pip install pylatex`


### Examples

```python
import numpy as np

from pylatex import Document, Section, Subsection, Table, Math, TikZ, Axis, \
    Plot, Figure, Package
from pylatex.numpy import Matrix
from pylatex.utils import italic, escape_latex

doc = Document()
doc.packages.append(Package('geometry', options=['tmargin=1cm',
                                                 'lmargin=10cm']))

with doc.create(Section('The simple stuff')):
    doc.append('Some regular text and some ' + italic('italic text. '))
    doc.append(escape_latex('\nAlso some crazy characters: $&#{}'))
    with doc.create(Subsection('Math that is incorrect')) as math:
        doc.append(Math(data=['2*3', '=', 9]))

    with doc.create(Subsection('Table of something')):
        with doc.create(Table('rc|cl')) as table:
            table.add_hline()
            table.add_row((1, 2, 3, 4))
            table.add_hline(1, 2)
            table.add_empty_row()
            table.add_row((4, 5, 6, 7))

a = np.array([[100, 10, 20]]).T
M = np.matrix([[2, 3, 4],
               [0, 0, 1],
               [0, 0, 2]])

with doc.create(Section('The fancy stuff')):
    with doc.create(Subsection('Correct matrix equations')):
        doc.append(Math(data=[Matrix(M), Matrix(a), '=', Matrix(M*a)]))

    with doc.create(Subsection('Beautiful graphs')):
        with doc.create(TikZ()):
            plot_options = 'height=6cm, width=6cm, grid=major'
            with doc.create(Axis(options=plot_options)) as plot:
                plot.append(Plot(name='model', func='-x^5 - 242'))

                coordinates = [
                    (-4.77778, 2027.60977),
                    (-3.55556, 347.84069),
                    (-2.33333, 22.58953),
                    (-1.11111, -493.50066),
                    (0.11111, 46.66082),
                    (1.33333, -205.56286),
                    (2.55556, -341.40638),
                    (3.77778, -1169.24780),
                    (5.00000, -3269.56775),
                ]

                plot.append(Plot(name='estimate', coordinates=coordinates))

    with doc.create(Subsection('Cute kitten pictures')):
        with doc.create(Figure(position='h!')) as kitten_pic:
            kitten_pic.add_image('docs/static/kitten.jpg', width='120px')
            kitten_pic.add_caption('Look it\'s on its back')

doc.generate_pdf()
```

This code will generate this:

![Generated PDF by PyLaTeX](https://raw.github.com/JelteF/PyLaTeX/master/docs/static/screenshot.png)


### Future development

I will keep adding functionality I need to this library.

If you add a feature yourself, or fix a bug, please send a pull request. The
code is not very difficult and mostly speaks for itself. If you have a question
just let me know.

You can submit issues and I will probably respond quite quick. However, it might
take a lot more time for me to fix something. I also have a job, education and a
personal life to worry about. If you want something done try to fix it yourself
as well. Accepting pull requests costs way less time.

### Support

This library is being developed in and for Python 3. Because of a conversion
script the current version also works in Python 2.7. For future versions, no
such promise will be made. Python 3 features that are useful but incompatible
with Python 2 will be used. If you find a bug for Python 2 and it is fixable
without ugly hacks feel free to send a pull request.

The platform this library is developed for is Linux. I have no intention to
write fixes or test for platform specific bugs with every update. Pull requests
that fix those issues are always welcome though.


### Copyright and License

Copyright 2014 Jelte Fennema, under [the MIT
license](https://github.com/JelteF/PyLaTeX/blob/master/LICENSE)
