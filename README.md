# IIB-Kondor-TBL-DFDL-Message-Model
DFDL Message model library for parsing Kondor DB export files in IBM Integration Bus.

The Kondor message is parsed into the field-value pairs in the DFDL message tree.

This library also contains a sub flow that will convert the Kondor text file to it's XMLNSC equivalent for convenience of use with Compute nodes.

**Note:** I have not included the `IBMdefined\GeneralPurposeFormat.xsd` file which is referenced by the message model because of IBM  copyright. If someone can confirm that its legal to include it here, I will do so. In the meanwhile, you can get this file from within IIB Toolkit using the steps below

**How to use:**

1. Clone the repository and import the project into IIB Toolkit (there will be build errors because of the missing `GeneralPurposeFormat.xsd`, this is to be expected. See Note above).
2. Reference the library into your project.
3. Drag and drop the subflow `SF_KONDOR_TEXT2XML` from the library into a message flow in your application.
4. Create a FileInput node and set the `Input Message Parsing` properties to use the `KONDOR_TBL` message model (should appear when you click Browse if the library is referenced correctly).
5. (Important) Set `Parse Timing` to Immediate or Complete to prevent a possible exception.
6. Connect this FileInput node to the input of the subflow.

The subflow will parse the DFDL message tree of the Kondor message and convert it to an XMLNSC domain message tree. Therefore, the invidual fields can be referenced using standard IIB Field Reference methods.

**Adding GeneralPurposeFormat.xsd**

Since `GeneralPurposeFormat.xsd` has not been included  to avoid a possible copyright issue. Until I can confirm that including it here is allowed, you must procure it by other means and copy it to `IBMdefined`. Fortunately, you can also reference the file within IIB Toolkit itself by following the below steps:
1. After importing the library into IIB Toolkit, open `KONDOR_TBL.xsd` in the DFDL editor.
2. In the `Representation Properties` on the right, find the property `Data Format Reference` and click on the button shown below:

![properties1.png](https://i.imgur.com/gxDXtkh.png)

3. In the `Reference Selection` dialog box that appears, select `GeneralPurposeFormat` if present, else click on `Add a reference to another schema` as shown below:

![properties2.png](https://i.imgur.com/N6u052v.png)

4. In the next dialog box, select `General purpose` then click Ok and then click Ok.

![properties3.png](https://i.imgur.com/FQxWg1r.png)

5. Save the DFDL message model. The build errors will be gone after your project auto-builds. If the error still persists, then try cleaning the project.

**Caveats:**

Please note the following points when using this message model:
1. The Parse Timing must always be Immediate or Complete in your respective input nodes that will read the Kondor file.
2. Since some Kondor fields start with numbers (eg. 360T), the subflow will create an XML field with the same name without XML validation. Therefore, if the XMLNSC message is output to a file, this file might not be valid XML since XML field names do not start with numerals. However, this message will be valid if processing only within IIB provided such fields are referenced using double-quotes (eg. `InputRoot.XMLNSC.KONDOR_TBL.UsersCodes."360T"`).
