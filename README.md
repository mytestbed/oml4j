# OML4J: Native OML Implemantation in Java #
This is a simple client for OML which does not use liboml2 and its filters, but connects directly to the server using the oml text protocol. User can use this library to create Java applications which can send measurements to the OML collection server. A usage description of the OML implementation and two examples are attached below. 

## Usage ##

This module provides the OMLBase class, which contains the following methods:

- init 
- start
- addmp
- inject
- close

To use OML in a java project you can either:
 
- import the omlBasePackage into your project. As far as eclipse is concerned,  perform :

        Import->General->Archive File->Browse : Choose the omlBasePackage.zip folder 
		Locate the extracted folder underneath the src path of project's directory tree.

- import the oml4j.jar file into your project. As far as eclipse is concerned, perform :

		Build Path->Add External Archives->Browse : Choose the oml4j.jar file
	
Then add the package to your project:

    import omlBasePackage.OMLBase;

Start by initializing an OMLBase object. The init method, in our case the object's constructor, takes up to four arguments:

- the name of the application,
- the name of the experiment,
- the name of the node,
- and the OML server URI in the form tcp:hostname:port

For example:

    OMLBase omlclient = new OMLBase("TestApp", "ioigoume-exp", "testapp", "tcp:nilab.inf.uth.gr:3003");

The only mandatory argument is the first one (the name of the application). If you skip any of the others, they may be defined by environment variables (OML_DOMAIN, OML_NAME, OML_COLLECT) or via command-line options. If these variables are not passed in explicitly and neither the command line options nor environment variables are defined then the application will run with OML disabled, and the measurements that would have been sent to OML will be printed on stdout instead.

Next, add one or more measurement points. Foreach measurement point create the appropriate object. The object's constructor takes two parameters. The first one is the name of the variable and the second one is the type of the variable. The valid variable types are predefined in an ENUM structure. For example:

    OMLMPFieldDef vt1 = new OMLMPFieldDef("counter",MPTYPES.OML_INT32_VALUE.getMtype());
    OMLMPFieldDef vt2 = new OMLMPFieldDef("name",MPTYPES.OML_STRING_VALUE.getMtype());
    OMLMPFieldDef vt3 = new OMLMPFieldDef("surname",MPTYPES.OML_STRING_VALUE.getMtype());
	OMLMPFieldDef vt4 = new OMLMPFieldDef("val1",MPTYPES.OML_INT32_VALUE.getMtype());
	OMLMPFieldDef vt5 = new OMLMPFieldDef("val2",MPTYPES.OML_INT32_VALUE.getMtype());


Next, use the measurement points you created and make the schema objects you want.


    OMLSchemaCST mp_1 = new OMLSchemaCST(new ArrayList<OMLMPFieldDef>(Arrays.asList(vt1,vt2,vt3)));
	OMLSchemaCST mp_2 = new OMLSchemaCST(new ArrayList<OMLMPFieldDef>(Arrays.asList(vt4,vt5)));

Each schema object can return the schema string according to the protocol version the user defines. Use this functionality and call the addmp function of the omlclient object to create the final schema structure. For example:

    omlclient.addmp("tbl1", mp_1.retSchema(1)); 
    omlclient.addmp("tbl2", mp_2.retSchema(1));



When you have set up all your measurement points, call start():

    omlclient.start();

For presentation purposes we will create a few hardcoded measurement tuples:

    String[] data = { "1", "Ioannis", "Igoumenos" };
    String [] data2 = { "3", "5"};

Since you have measurement points ready to send to OML, use the inject function as follows:


    omlclient.inject("tbl1", data);
    omlclient.inject("tbl2", data2);


At the end of your program, call close to gracefully close the database:


    omlclient.close();


## Examples ##

You can find a native java and an android example inside the repository. The first one uses the jar approach and the second one the package approach in loading the OMLBase class.

## License ##

Copyright 2013 NITLab, University of Thessaly, CERTH, Greece

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.