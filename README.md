# RTA-base-GUI

This repository contains the new widgets for the rta scientific gui that is based on the [https://github.com/IftachSadeh/ctaOperatorGUI](Iftach's ctaOperatorGUI).

## Development flow
* Open branch
* Develop
  * Import a new web component from RTA-GUI-components in ctaOperatorGUI
    * copy the component in the template folder
    * import the component file in the template/view_common.jinja2 file
    * create a new widget
    * add the new widget to the allowed widgets (in ctaGUIUtils/py/utils.py)
    * add the new widget to the view 300 (in js/utils_setupView.js)
  * Update the copyTo/copyFrom scripts
  * Run the scripts in order to import here the new files
* Commit and push
* Open pull request
* When accepted
  * Checkout on master
  * git pull
  * delete branch

## How to merge the ctaOperatorGUI with the new widgets
- Clone the Iftach's ctaOperatorGUI with:
  ```bash
    git clone https://github.com/IftachSadeh/ctaOperatorGUI
  ```
- Call the following script that copy all the files present in this repo in the ctaOperatorGUI. The script takes in input the absolute path to the root folder of the ctaOperatorGUI.
  ```bash
    ./copyTo.sh <absolute-path-to-ctaOperatorGUI-dir>
  ```
  The script will copy:
  - the external web-components created with Polymer and Plotly (.html).
  - the new custom widgets (.js and .py).
  - the plotly css stylesheet.

- Modify the following files:
  - ctaGuiFront/bower.json
    - Add "plotly.js": "^1.39.3" to the dependencies list

  - ctaGuiFront/ctaGuiFront/templates/view_common.jinja2
    - For each external web-component copied inside the /ctaGuiFront/ctaGuiFront/template folder, import each web-component with:
      ```html
        <link rel="import" href="/templates/<component name>.html">
      ```
  - ctaGuiUtils/py/utils.py
    - Add to the 'allowedWidgetTypeV' object, within the 'synced' list, the name of all the new custom widgets.

  - ctaGuiFront/ctaGuiFront/js/utils_setupView.js
    - Build the views of the rta scientific GUI by defining each view and by adding widgets to them. See code for examples.

  - ctaGuiFront/ctaGuiFront/__init__.py
    - Add the views to the utils.allWidgets list.

  - ctaGuiFront/ctaGuiFront/templates/utils_webComp.html
    - Add the home page links to the views. If the links are added to the <paper-listbox id="siteNavMenu"> tag, every users can see them. It is possible to hide certains links to 'guest' users and make them visible only for the 'developer' users. In order to do so, create the link dynamically in the addDevItem(nTry), see code for examples.

    - ctaGuiFront/ctaGuiFront/templates/utils_widgets.html
      - Rename the 'CTA operator GUI' titles with custom ones. Search for those strings.

    - ctaGuiFront/ctaGuiFront/templates/view_common.jinja2
      - Rename the 'CTA' title tag with a custom one.

    - (optional) ctaGuiUtils/py/utils_redis.py
      - If you are using Redis with authentication, give the password to the Redis connection method:
      ```python
        self.redis = redis.StrictRedis(host=host, port=port, db=3, password=redisPassword)
      ```

- Rename the ctaOperatorGUI folder.


## Add a new plotly widget in the ctaOperatorGUI
```bash
tag0="plotlyWebComp"
tag1="plotlywebcomp"
tag2="PlotlyWebComp"

cd ctaGuiFront/ctaGuiFront
sed "s/emptyPlotlyExample/${tag0}/g" js/widget_emptyPlotlyExample.js | sed "s/emptyplotlyexample/${tag1}/g" | sed "s/EmptyPlotlyExample/${tag2}/g" > js/widget_${tag0}.js
sed "s/emptyPlotlyExample/${tag0}/g" py/widget_emptyPlotlyExample.py | sed "s/emptyplotlyexample/${tag1}/g" | sed "s/EmptyPlotlyExample/${tag2}/g" > py/widget_${tag0}.py
```
