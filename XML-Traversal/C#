
//Names for nodes to scan for
string XMLNodeName = "entry";
//A class scoped variable to increment an arbitrary surrogate key for each valid scanned record.
int uid;

//Function: Read through the XML doc specified in _GetTOCFilepath() and attemtp to build a database from it. If an exception occurs, catch exception and build an error message.
protected void BTN_genXML_Click(object sender, System.EventArgs e)
{
	try {
		_XMLTraversalAndBuilder();
	} catch (Exception ex) {
		//errortext = _getlinepreview(_GetTOCFileLocation, CType(L_linenum.Text, Integer))
	}
}

//Function: access the broken file and return a preview of the error generated from _XMLTRAVERSALANDBUILDER
private string _getlinepreview(string filepath, int linenum)
{
	string s = "";
	using (StreamReader File = new StreamReader(_GetTOCFileLocation())) {
		for (int i = 1; i <= linenum - 2; i++) {
			if ((File.ReadLine() == null)) {
				throw new ArgumentOutOfRangeException("Line Number is not valid, ensure the file is not empty");
			}
		}
		try {
			string line = null;
			for (int i = 1; i <= 3; i++) {
				line = File.ReadLine();
				if (line == null) {
					throw new ArgumentOutOfRangeException("Line Number is not valid, ensure the file is not empty");
				}
				line = line.Replace("<", "&lt;").Replace(">", "&gt;");
				if ((i == 2)) {
					line = "<strong>" + line + "</strong>";
				}
				s = s + line + "<br />";
			}
		} catch (Exception ex) {
		}
	}
	return s;
}

//Function: Load the XML document, and begin lateral traversal. 
private void _XMLTraversalAndBuilder()
{
	XmlDocument xmlDoc = new XmlDocument();
	using (FileStream fs = new FileStream(_GetTOCFileLocation(), FileMode.Open, FileAccess.Read, FileShare.Read)) {
		xmlDoc.Load(fs);
		XmlNode root = xmlDoc.DocumentElement;
		_scanChildRecursive(root, 0, uid);
	}
	//Show success
}

//Function to return file when loaded. abstracted if we need to go cross-server etc.
private string _GetTOCFileLocation()
{
	return Server.MapPath("toc.xml");
}

//Recursive depth-breadth node search
private void _scanChildRecursive(XmlNode node, int level, int parentid)
{
	foreach (XmlNode n in node.ChildNodes) {
		if ((n.Name.ToLower() == XMLNodeName)) {
			_GetNodeData(ref n, level, parentid);
		}
		if ((n.HasChildNodes)) {
			//Use logic here to remove nodes you do not with to scan, i.e. disabled=true etc. 
			_scanChildRecursive(n, level + 1, uid);
		}
	}
}

private void _GetNodeData(ref XmlNode xml, int level, int parentid)
{
	//Logic for removing potentially invalid records from bieng added, i.e. duplicate values, etc. 
	string label = xml.Attributes("label") == null ? xml.Attributes("layer").Value : xml.Attributes("label").Value;
	uid += 1;
	_InsertRecord(uid, label, parentid, xml.Attributes("id").Value);
}

private void _InsertRecord(int uid, string label, int parentid, string l_id)
{
	//Add to records system
}
