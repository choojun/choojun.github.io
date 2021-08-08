Suppose that you do not like cloud computing, and the alternative for u is this virtual grid computing. This is an option that you can consider for **grepping** more resources. 

I come across this [Volunteer computing](http://boinc.berkeley.edu/trac/wiki/VolunteerComputing), BONIC (a opensource server that can be setup) at URL http://boinc.berkeley.edu/projects.php, and saw them hosted a few Quantum computing and Biology projects. 

It uses the [Virtual Campus Super computer Center](http://boinc.berkeley.edu/trac/wiki/VirtualCampusSupercomputerCenter) concept to gather more computational resources (including lab machines, desktop and laptops belonging to faculty, staff, and students, and home PCs belonging to university and the public). Those volunteers' machine just need to install the [client](http://boinc.berkeley.edu/download.php) and the BONIC server can "tab" their machine's resources (imaging during university's semester break, and they are **tabbing** all PC in main campus.... it is going to be a lot lot resources, hahaha!). 

**But i foresee one problem for those who are non-compueter science backgroud** where u need to write BONIC application in order to make use those **donated** resources regardless any operating system from volunteers. 

It supports [multi-thread](http://boinc.berkeley.edu/trac/wiki/AppMultiThread), and your program that require GPU (example [1](http://boinc.berkeley.edu/trac/wiki/AppCoprocessor) and [2](http://boinc.berkeley.edu/trac/wiki/CudaApps)) which all in C, or [Java](http://boinc.berkeley.edu/trac/wiki/JavaApps), [Fortran](http://boinc.berkeley.edu/trac/wiki/FortranApps) too! More important is your application can make use specifically Cuda or [OpenCL](http://boinc.berkeley.edu/trac/wiki/GPUApp). 

Once done yr program development, submit yr job ( example [simple Job](http://boinc.berkeley.edu/trac/wiki/SingleJob) or [remote Job](http://boinc.berkeley.edu/trac/wiki/RemoteJob) ) to BONIC server for execution. The requirement to setup a BONIC server is available at URL http://boinc.berkeley.edu/trac/wiki/ServerIntro. 
