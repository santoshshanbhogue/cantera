Cantera Development Practices

Harry Moffat

Draft -3 3/13/2014

### Project Planning

Project planning must be carried out.
Plans for projects should be implemented at a higher conceptual level. They should include a gross outline of what new classes are to be constructed and what theory those classes will implement. The plans should be submitted to the Developers site, and gross agreement amongst the developers for the project and the structure of its implementation should be agreed to before the project is launched.
When a new project is launched, if it involves a large departure from the existing formalism, development of the project should occur along a development branch if it involves algorithm changes for common capabilities or if it involves a significant amount of development. When the code is stable, a merge should be done to synch up with the main branch.
A development project is made up of a bunch of development modules. Each development module should have a testing capability attached to it of some sort. A development module may be included directly in the main development branch, if it makes sense to do it there. A development module has a documentation requirement. A memo must be produced that has the equation set, the algorithm employed, and an example employing the capability. Tutorial formats are preferred. This information should also be added to the Doxygen information in the file for the module.

### Doxygen documentation

It is expected that Doxygen will be the main documentation system for Cantera. All capabilities within Cantera and all formulas that are used should be documented within the Doxygen implementation. Where formulas become too gnarly, (for example, the Dixon-Lewis multicomponent transport equations for dilute gases) outside references may used as long as explicit references to formulas within the outside documentation are provided. Doxygen can’t handle all of the documentation needs of the program. It is expected that how-to-tutorials will be added as they get constructed. Additionally journal articles will provide additional documentation sources. A permanent site for the Doxygen pages will be located.
Doxygen documentation should be carried out concurrently with the initial development effort.  A clean Doxygen build should be the goal at the time of all checkins. An effort to completely document the existing code base using Doxygen is underway. However, it’s a large undertaking, and we are only 30% there.
Meta-information within Doxygen should be implemented as well as part of the development, testing, and validation cycle for new objects. Meta-information here is defined as high level pages within Doxygen that describe new functionality and how it is categorized. Additionally, the detailed formulas for implementation of the functionality and the description of the algorithm is categorized as meta-information.
Detailed Doxygen information is defined as the descriptions of member functions and member data that go into the code and that are needed to prevent warning messages from appearing during the Doxygen page formation.

### Release Cycle

We would like to release Cantera at yearly intervals. At that time, a release branch will be created. Bug fixes will be included along that release branch. At some point, releases will not further supported. A list of new functionality will be provided with each release at the time of the release.

### Testing Cycle

We have started a unit testing capability that is also located at Google code. Right now, there are only a few tests in the suite. We hope to expand the testing capability substantially. Right now, it’s based on a “make test” capability using a blessed output file. However, we will be looking into using other tests such as ctest or Google Test.
Each object should have multiple units tests associated with it that test the fine grained implementation of the object. Tests that check the agreement with theory are preferred. If there is substantial number of derivatives taken tests that check against numerical differencing scheme are required.
Tests that check the duplication functions, assignment operators, and copy constructors are required as well.

We are migrating to a style where tests are modular, i.e., everything contained in a single directory, and contain explicit documentation concerning the test. This type of style is needed for verification and validation of models. In other words, Cantera deals with numbers, and therefore its verification needs should involve the explicit text statement of what’s to be calculated from Cantera, what’s actually calculated from Cantera, the equations that are used to calculate the quantity, the literature references on the subject, and how all of this has changed as a function of time. And, this must be done outside of the blessed file approach, so that inadvertent changes in blessed files don’t cause a drift of Cantera away from the original verification study carried out by the original developer. Note, this type of error has been documented to occur with other Sandia codes in a multi-developer environments.

Therefore, each test directory should contain:

- README containing a description of the test along with the original validation information.
- runtest script to run the test
- data files for running the script -> all included unless we are testing data repository retrieval.
- History of the deltas and changes of the values as a function of the history
- Blessed files
- optional Graphics input for creation of a visual plot. Nothing like a plot to gauge the importance of changes to the results.

It’s expected that the process of building up this test database will be incremental.
Note this test environment is primarily designed for verification and tutorial purposes, and not necessarily for development deltas.


### Coding Style

- Large changes should be split into small, stand-alone commits
- Each commit should represent changes for a single purpose

This makes reviewing easier because it reduces the number and scope of changes to inspect

New code should be written that adheres to the original style of Cantera. This is to promote readability and to prevent refactorization from occurring solely due to coding style preferences. The basic style that we are using adheres to the following web site:
http://geosoft.no/development/cppstyle.html
another reference is
http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml
Additionally, we seek to stay away from templated code and from using complex C++ implementation wherever possible, even if that means duplication of code or the length of the code increases. Our aim is to enable unsophisticated scientists and engineers to make contributions to the code base. This requires that the layout be straightforward and understandable at the intermediate C++ expertise level.
C pointer arithmetic is to be preferred over its more complex C++ equivalents using algorithm std implementations.

### Coding Style Transitions under way:

There are several coding style transitions under way. We do not have the time spend project time solely for this purpose, so we do it over a period of time. Let’s go through them.

1. Traditionally, member data had been prefixed with m_ to indicate that it is a member of a class:
  e.g.: `m_temperature`
We are transitioning to a single underscore suffix to denote membership in a class:
   e.g.: `temperature_`
2. We are transitioning to a 120 line length. Each function in a file is separated by a line of the following form:

  //======================================================================

  Note, this is the format that is used by Trilinos. Comments are to be used everywhere to enhance the understanding and readability of the code, even if they are duplicated.

3. We are transitioning to a system where all definitions of functions occur in .cpp files. Header files only include declarations and Doxygen documentation.
4) We are transitioning to the capability that all lower level objects should be able to be duplicated using copy constructors. This allows for interesting capabilities.

### Peer Review

We have not implemented peer review in any meaningful way. Googlecode has a peer review capability. However, it does have its beneficial aspects:

- be seen as beneficial to both reviewers and authors (either may learn, and knowledge is shared)
- increase the quality of our code by:
  * avoiding bugs
  * avoiding duplication of functionality
  * avoiding inconsistent implementations and incompatible features
  * encouraging consistent code style (formatting, naming conventions, etc.)
  * encouraging good design practices
- ensure that the general philosophy of Cantera is adhered to when new development occurs.

It does have its drawbacks. We should guard against:
- not decreasing our long term performance in terms of creating usable capability
- be a conduit for code style wars.
- rewriting other’s code snippets just for code style.
