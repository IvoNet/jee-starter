Welcome to the official Eclipse Foundation starter for Jakarta EE. The starter is a Maven Archetype that generates
sample code to get you going quickly with simple Jakarta EE microservices projects. The starter will include a web UI in
a subsequent release.

## Generate Jakarta EE Project

In order to run the Maven Archetype and generate Jakarta EE project, fill in the form and the correct command will be
generated. Please make sure you have [Maven 3+](https://maven.apache.org/download.cgi) installed and configured
correctly.

<script>
function isEmpty(element) {
    return element.value === undefined || element.value === "undefined"|| element.value.trim() === "";
}

function generateMvnCommand() {
    const mavenArchetype = document.getElementById("mavenArchetype");
    const mvnArchetypeGenerate = document.getElementById("mvnArchetypeGenerate");
    const { mvnArchetypeGroupId, mvnArchetypeArtifactId, mvnArchetypeVersion } = mavenArchetype.value.split(",");
    const groupId = document.getElementById("groupId").value;
    const artifactId = document.getElementById("artifactId").value;
    const projectVersion = document.getElementById("projectVersion").value;

    mvnArchetypeGenerate.value = `mvn archetype:generate -DarchetypeGroupId=${mvnArchetypeGroupId} -DarchetypeArtifactId=${mvnArchetypeArtifactId} -DarchetypeVersion=${mvnArchetypeVersion} -DgroupId=${groupId} -DartifactId=${artifactId} -Dversion=${projectVersion}`;
}

function removeMvnCommand() {
    const mvnArchetypeGenerate = document.getElementById("mvnArchetypeGenerate");
    mvnArchetypeGenerate.value = "";
}

function copyMvnCommand() {
    const mvnArchetypeGenerate = document.getElementById("mvnArchetypeGenerate");
    mvnArchetypeGenerate.select();
    mvnArchetypeGenerate.setSelectionRange(0, 99999);
    navigator.clipboard.writeText(document.getElementById("mvnArchetypeGenerate").value);
}

class FormValidator {
  constructor(form, fields) {
    this.form = form;
    this.fields = fields
  }

  initialize() {
    this.validateOnEntry();
    this.validateOnChange();
  }

  validateOnChange() {
    let self = this;

    this.form.addEventListener('change', event => {
        // Disabled to see if the regular HTML5 browser validation would trigger again.
        // event.preventDefault();
        self.fields.forEach(field => {
        const input = document.getElementById(field);
        self.validateFields(input)
      })
    })
  }

  validateOnEntry() {
    let self = this;
    this.fields.forEach(field => {
      const input = document.getElementById(field);

      input.addEventListener('change', event => {
        self.validateFields(input)
      })
    })
  }

  validateFields(field) {
    if (field.type === "text") {
      if (isEmpty(field)){
        this.setStatus(field, `${field.previousElementSibling.innerText} must be filled`, "error");
      }else {
          this.setStatus(field, null, "success");
      }
      if (field.value.includes(" ")) {
        this.setStatus(field, `${field.previousElementSibling.innerText} cannot contain spaces`, "error")
      } else {
        this.setStatus(field, null, "success")
      }
    }    
  }

  setStatus(field, message, status) {
    const errorMessage = field.parentElement.querySelector('.error-message');
    if (status === "success") {
      if (errorMessage) { errorMessage.innerText = "" }
      field.style = "border-color: #ccc;";
      generateMvnCommand();
    }

    if (status === "error") {
      field.parentElement.querySelector('.error-message').innerText = message;
      field.style = "border-color: red;";
      removeMvnCommand();
    }
  }

}
</script>

<form id="archetypeUI">
    <div class="form-row">
        <div class="form-group" >
            <label for="mavenArchetype">Archetype</label>
            <select class="form-control" id="mavenArchetype">
                <option value="org.eclipse.starter,jakartaee10-minimal,1.0.0">Jakarta EE 10 Minimal Archetype</option>
                <option value="org.eclipse.starter,jakartaee9.1-minimal,1.0.0">Jakarta EE 9 Minimal Archetype</option>
                <option value="org.eclipse.starter,jakartaee8-minimal,1.0.0">Jakarta EE 8 Minimal Archetype</option>
                <option value="nl.ivonet,jakartaee8-minimal,1.0.0">IvoNet - Jakarta EE 8 Minimal Archetype </option>
            </select>
        </div>
    </div>
    <div class="form-row">
        <div class="form-group">
            <label for="groupId">Group</label>
            <input class="form-control" type="text" id="groupId" placeholder="com.example" pattern="[a-z][a-z0-9_]*(\.[a-z0-9_]+)+[0-9a-z_]$" minlength="1" title="Group should only contain lowercase letters and be dot-separated.">
            <span class="error-message"></span> 
        </div>
        <div class="form-group">
            <label for="artifactId">Artifact</label>
            <input type="text" class="form-control" id="artifactId" placeholder="demo" pattern="[a-z]+(-[a-z]+)*$" minlength="1" title="ArtifactId should only contain lowercase letters and hyphens (-).">
            <span class="error-message"></span> 
        </div>
        <div class="form-group">
            <label for="projectVersion">Version</label>
            <input type="text" class="form-control" id="projectVersion" placeholder="1.0-SNAPSHOT" minlength="1">
            <span class="error-message"></span> 
        </div>
    </div>
    <div class="form-group">
        <label for="mvnArchetypeGenerate">
            Just copy this command as is in a terminal where you want to start your project and press enter...
        </label>
        <textarea class="form-control"
                  id="mvnArchetypeGenerate"
                  rows="11"
                  readonly
                  aria-describedby="mvnCommandHelp"
                  onclick="copyMvnCommand()">
        </textarea>
        <small id="mvnCommandHelp" class="form-text text-muted">By clicking on the text it will be copied to the
            clipboard
        </small>
    </div>
</form>

<script>
  const form = document.getElementById('archetypeUI');
  const fields = ["groupId", "artifactId", "projectVersion"];
  
  const validator = new FormValidator(form, fields);
  validator.initialize();
  generateMvnCommand();
</script>

## Generate Jakarta EE Sample Project

In order to run the Maven Archetype and generate a sample Jakarta EE project, please execute the following. Please
ensure you have installed a [Java SE 8+ implementation](https://adoptium.net/?variant=openjdk8)
and [Maven 3+](https://maven.apache.org/download.cgi) (we have tested with Java SE 8, Java SE 11 and Java SE 17).

## Example projects

If desired, you can easily use the Maven Archetype from a Maven capable IDE such
as [Eclipse](https://www.eclipse.org/ide).

If you use the defaults, this will generate the Jakarta EE project under a directory named `jakartaee-cafe`. You can
then run the project by executing the following command from the `jakartaee-cafe` directory. Please ensure you have
installed a [Java SE 8+ implementation](https://adoptium.net/?variant=openjdk8)
and [Maven 3+](https://maven.apache.org/download.cgi) (we have tested with Java SE 8, Java SE 11 and Java SE 17).

```
mvn clean package payara-micro:start
```

Once Payara Micro starts, you can access the project at http://localhost:8080.

You can also run the project via Docker. To build the Docker image, execute the following commands from
the `jakartaee-cafe` directory. Please ensure you have installed
a [Java SE 8+ implementation](https://adoptium.net/?variant=openjdk8), [Maven 3+](https://maven.apache.org/download.cgi)
and [Docker](https://docs.docker.com/get-docker/) (we have tested with Java SE 8, Java SE 11 and Java SE 17).

```
mvn clean package
docker build -t jakartaee-cafe:v1 .
```

You can then run the Docker image by executing:

```
docker run -it --rm -p 8080:8080 jakartaee-cafe:v1
```

Once Payara starts, you can access the project at http://localhost:8080/jakartaee-cafe.

The generated starter code is simply a Maven project. You can easily load, explore and run the code in a Maven capable
IDE such as [Eclipse](https://www.eclipse.org/ide).

We hope you enjoy your Jakarta EE journey!

## Learn more!

There are many excellent free resources to learn Jakarta EE! The following are some that you should begin exploring
alongside the starter.

* The [Jakarta EE Tutorial](https://eclipse-ee4j.github.io/jakartaee-tutorial) is a comprehensive reference for
  developing applications with Jakarta EE.
* The [First Cup](https://eclipse-ee4j.github.io/jakartaee-firstcup/) is part of the Tutorial and is a gentle hands-on
  introduction to Jakarta EE.
* You can further explore the [First Cup Examples](https://github.com/eclipse-ee4j/jakartaee-firstcup-examples) to get a
  feel for how Jakarta EE applications look like.
* The [Jakarta EE Tutorial Examples](https://github.com/eclipse-ee4j/jakartaee-tutorial-examples) is a very
  comprehensive resource showing you how to use many Jakarta EE APIs and features.
* The [Cargo Tracker](https://eclipse-ee4j.github.io/cargotracker/) project demonstrates first-hand how you can develop
  applications with Jakarta EE using widely adopted architectural best practices like Domain-Driven Design (DDD).
