<?jelly escape-by-default='true'?>
<j:jelly xmlns:j="jelly:core" xmlns:st="jelly:stapler" xmlns:d="jelly:define">
<html>
  <head>
      <title>${PROJECT_NAME}</title>
      <style>
          body table, td, th, p, h1, h2 {
          margin:0;
          font:normal normal 100% Georgia, Serif;
          background-color: #ffffff;
          }
          h1, h2 {
          border-bottom:dotted 1px #999999;
          padding:5px;
          margin-top:10px;
          margin-bottom:10px;
          color: #000000;
          font: normal bold 130% Georgia,Serif;
          background-color:#f0f0f0;
          }
          tr.gray {
          background-color:#f0f0f0;
          }
          h2 {
          padding:5px;
          margin-top:5px;
          margin-bottom:5px;
          font: italic bold 110% Georgia,Serif;
          }
          .bg2 {
          color:black;
          background-color:#E0E0E0;
          font-size:110%
          }
          th {
          font-weight: bold;
          }
          tr, td, th {
          padding:2px;
          }
          td.test_passed {
          color:blue;
          }
          td.test_failed {
          color:red;
          }
          td.test_skipped {
          color:grey;
          }
          .console {
          font: normal normal 90% Courier New, monotype;
          padding:0px;
          margin:0px;
          }
          div.content, div.header {
          background: #ffffff;
          border: dotted
          1px #666;
          margin: 2px;
          content: 2px;
          padding: 2px;
          }
          table.border, th.border, td.border {
          border: 1px solid black;
          border-collapse:collapse;
          }
          td.right {
          text-align:right;
          }
      </style>
  </head>
  <body>
    <div class="header">
      <j:set var="spc" value="&amp;nbsp;&amp;nbsp;" />
      <!-- GENERAL INFO -->
	  <h1>General Info</h1>
      <table>
        <tr class="gray">
          <td align="right">
            <j:choose>
              <j:when test="${BUILD_STATUS=='SUCCESS'}">
                <img src="${BUILD_URL}/war/images/32x32/blue.gif" />
              </j:when>
              <j:when test="${BUILD_STATUS=='FAILURE'}">
                <img src="${BUILD_URL}/war/images/32x32/red.gif" />
              </j:when>
              <j:otherwise>
                <img src="${BUILD_URL}/war/images/32x32/yellow.gif" />
              </j:otherwise>
            </j:choose>
          </td>
          <td valign="center">
            <b style="font-size: 200%;">Base24 UAT Regession. Build Number:${BUILD_NUMBER}, Build Status:${BUILD_STATUS}</b>
          </td>
        </tr>
        <tr>
          <td>Project Name:</td>
          <td>${PROJECT_NAME}</td>
        </tr>
		<tr>
          <td>Project URL:</td>
          <td>${PROJECT_URL}</td>
        </tr>
		<tr>
          <td>Project Default Content:</td>
          <td>${PROJECT_DEFAULT_CONTENT}</td>
        </tr>
		<tr>
          <td>Project Default Subject:</td>
          <td>${PROJECT_DEFAULT_SUBJECT}</td>
        </tr>
        <tr>
          <td>Date of build:</td>
          <td>${it.timestampString}</td>
        </tr>
      </table>
    </div>

    <!-- HEALTH TEMPLATE -->
    <div class="content">
      <j:set var="healthIconSize" value="16x16" />
      <j:set var="healthReports" value="${project.buildHealthReports}" />
      <j:if test="${healthReports!=null}">
        <h1>Build Info</h1>
        <table>
		<tr>
          <td>Build URL</td>
          <td>
            <a href="${BUILD_URL}">Please click here!</a>
          </td>
        </tr>
        <tr>
          <td>Build duration:</td>
          <td>${build.durationString}</td>
        </tr>
        <tr>
          <td>Build cause:</td>
          <td>
            <j:forEach var="cause" items="${BUILD_CAUSE}">${cause.shortDescription} </j:forEach>
          </td>
        </tr>
        <tr>
          <td>Build description:</td>
          <td>${JOB_DESCRIPTION}</td>
        </tr>
        <tr>
          <td>Built on:</td>
          <td>
            <j:choose>
              <j:when test="${build.builtOnStr!=''}">${build.builtOnStr}</j:when>
              <j:otherwise>master</j:otherwise>
            </j:choose>
          </td>
        </tr>
        </table>
        <br />
      </j:if>
    </div>

    <!-- CHANGE SET -->
    <div class="content">
      <j:set var="changeSet" value="${build.changeSet}" />
      <j:if test="${changeSet!=null}">
        <j:set var="hadChanges" value="false" />
        <a href="${BUILD_URL}${build.url}/changes">
          <h1>Changes</h1>
        </a>
        <j:forEach var="cs" items="${changeSet.logs}" varStatus="loop">
          <j:set var="hadChanges" value="true" />
          <h2>${cs.msgAnnotated}</h2>
          <p>by <em>${cs.author}</em></p>
          <table>
            <j:forEach var="p" items="${cs.affectedFiles}">
              <tr>
                <td width="10%">${spc}${p.editType.name}</td>
                <td>
                  <tt>${p.path}</tt>
                </td>
              </tr>
            </j:forEach>
          </table>
        </j:forEach>
        <j:if test="${!hadChanges}">
          <p>No Changes</p>
        </j:if>
        <br />
      </j:if>
	  <table>
	     <tr>
          <td>Changes:</td>
          <td>${CHANGES}</td>
        </tr>
		<tr>
          <td>Changes since last success:</td>
          <td>${CHANGES_SINCE_LAST_SUCCESS}</td>
        </tr>
		<tr>
          <td>Changes since last unstable:</td>
          <td>${CHANGES_SINCE_LAST_UNSTABLE}</td>
        </tr>
		<tr>
          <td>Changes since last build:</td>
          <td>${CHANGES_SINCE_LAST_BUILD}</td>
        </tr>
	  </table>
    </div>

    <!-- ARTIFACTS -->
    <j:set var="artifacts" value="${build.artifacts}" />
    <j:if test="${artifacts!=null and artifacts.size()&gt;0}">
      <div class="content">
        <h1>Build Artifacts</h1>
        <ul>
          <j:forEach var="f" items="${artifacts}">
            <li>
              <a href="${BUILD_LOG_EXCERPT}">BUILD LOG EXCERPT</a>
            </li>
			<li>
              <a href="${BUILD_LOG}">BUILD LOG</a>
            </li>
			<li>
              <a href="${BUILD_LOG_MULTILINE_REGEX}">BUILD LOG MULTILINE REGEX</a>
            </li>
			<li>
              <a href="${BUILD_LOG_REGEX}">BUILD LOG REGEX</a>
            </li>
			<li>
              <a href="${LOG_REGEX}">LOG REGEX</a>
            </li>
          </j:forEach>
        </ul>
      </div>
    </j:if>
    </j:if>

    <div class="content">
      <!-- CONSOLE OUTPUT -->
      <a href="${BUILD_URL}/${LOG_REGEX}">
        <h1>Console Output</h1>
      </a>
      <table class="console">
        <j:forEach var="line" items="${build.getLog(50)}">
        <tr>
            <td><tt>${line}</tt></td>
        </tr>
        </j:forEach>
      </table>
      <br />
    </div>
  </body>
</html>
</j:jelly>


