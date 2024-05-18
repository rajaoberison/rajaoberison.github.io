# Tool to Add Distance to Coastline from CSV file
Using contours data and an estimate of the coastline elevation, this tool will help in adding distance value of point data coordinates to the created coastline. To use the tool, the user have to download it from here. Then, open the .tbx in ArcGIS. Expand the .tbx file and right click on the tool called: “Add Distance to Coastline from CSV file.tbx”. Go to Properties > Source, and point the .py file from the download.

![](https://paper-attachments.dropbox.com/s_A8644DBF08192B3C836A07BDBF5207209C6DE632DD90A438998B40149BBD3F31_1561142887461_Screen+Shot+2019-06-21+at+14.47.50+PM.png)


If, for some reason, the .tbx doesn’t work, it can be re-created by right-clicking in ArcCatalog > New > Toolbox. Right-click on the new tool, go to Parameters tab and add the parameters below.

| DISPLAY NAME                    | DATA TYPE         | PROPERTY > DIRECTION > VALUE | PROPERTY > DEFAULT > VALUE | PROPERTY > OBTAINED FROM > VALUE | PROPERTY > FILTER > VALUE | Description                                                                                                                                                                                                                                                                                                                                                        |
| ------------------------------- | ----------------- | ---------------------------- | -------------------------- | -------------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Pre-created Contours from DEM   | Feature Class     | Input                        |                            |                                  |                           | Pre-created contours. You can also create contours using a digital elavation model and ArcGIS' nternal tool called "Contour", available with the Spatial Analyst license.                                                                                                                                                                                          |
| Field containing contour values | Field             | Input                        |                            | Pre-created Contours from DEM    |                           |                                                                                                                                                                                                                                                                                                                                                                    |
| Estimate of coastline elevation | Double            | Input                        | 0                          |                                  |                           |                                                                                                                                                                                                                                                                                                                                                                    |
| Output Coordinate System        | Coordinate System | Input (Optional)             |                            |                                  |                           | The coordinate system you will be working on.                                                                                                                                                                                                                                                                                                                      |
| Linear Unit                     | String            | Input                        |                            |                                  | FEET_US, METERS, MILES_US | The unit used in the calculation. Make sure this match the coordinate system and the smoothing parameters if it's "checked".                                                                                                                                                                                                                                       |
| Point CSV file                  | File              | Input                        |                            |                                  |                           | The CSV file containing Latitude and Longitude of points.                                                                                                                                                                                                                                                                                                          |
| Longitude column                | Field             | Input                        | Longitude                  |                                  |                           |                                                                                                                                                                                                                                                                                                                                                                    |
| Latitude column                 | Field             | Input                        | Latitude                   |                                  |                           |                                                                                                                                                                                                                                                                                                                                                                    |
| Smooth coastline                | Boolean           | Input                        | False                      |                                  |                           | To choose whether to smooth the coastline of not:<br><br>- Checked: A PAEK smoothing will be applied to the coastline, based on the parameters specified. Small / short contour lines will also be removed. This could be very helpful if the contour was created from high resolution Lidar DEM (cf. figure below).<br>- Unchecked: No smoothing will be applied. |
| Smoothing threshold             | Linear Unit       | Input (Optional)             |                            | Pre-created Contours from DEM    |                           | The minimum length of line that you will keep for smoothing. The unit should be based on the coordinate system you'll be working on.                                                                                                                                                                                                                               |
| PAEK Smoothing parameter        | Linear Unit       | Input (Optional)             |                            | Pre-created Contours from DEM    |                           | PAEK parameter for smoothing. The unit should be based on the coordinate system you'll be working on.                                                                                                                                                                                                                                                              |
| Set Workspace Directory         | Workspace         | Input                        |                            |                                  |                           |                                                                                                                                                                                                                                                                                                                                                                    |

Then go to Validation tab, and replace the default with:

    import arcpy
    class ToolValidator(object):
      """Class for validating a tool's parameter values and controlling
      the behavior of the tool's dialog."""
    
      def __init__(self):
        """Setup arcpy and the list of tool parameters."""
        self.params = arcpy.GetParameterInfo()
    
      def initializeParameters(self):
        """Refine the properties of a tool's parameters.  This method is
        called when the tool is opened."""
        return
    
      def updateParameters(self):
        """Modify the values and properties of parameters before internal
        validation is performed.  This method is called whenever a parameter
        has been changed."""
        if self.params[8].value:
            self.params[9].enabled = True
            self.params[10].enabled = True
        else:
            self.params[9].enabled = False
            self.params[10].enabled = False
        return
    
      def updateMessages(self):
        """Modify the messages created by internal validation for each tool
        parameter.  This method is called after internal validation."""
        return


![Screenshot of the tool and User inputs](https://paper-attachments.dropbox.com/s_A8644DBF08192B3C836A07BDBF5207209C6DE632DD90A438998B40149BBD3F31_1561666159117_Screen+Shot+2019-06-27+at+16.08.22+PM.png)


Here is an image showing a cleaned and uncleaned coastline. The black lines will remain if the cleaning was not applied.

![](https://paper-attachments.dropbox.com/s_A8644DBF08192B3C836A07BDBF5207209C6DE632DD90A438998B40149BBD3F31_1561141080248_Screen+Shot+2019-06-21+at+13.24.36+PM.png)


