<!--- Copyright (C) 2009-2016 Lightbend Inc. <https://www.lightbend.com> -->
# Tercih Ettiğiniz IDE'yi kurmak

Play ile çalışmak kolyadır. Karmaşık bir IDE'ye ihtiyacınız bile yok, çünkü play otomatik olarak kaynak kodlardaki değişikliklerinizi derler ve yeniler, bu yüzden basit bir metin editörü kullanarak çalışabilirsiniz.

Buna rağmen, Modern bir Java ya da Scala IDE'si kullanmak otomatik tamamlama, anında derleme, desteklenmiş yeniden düzenleme ve debug etme gibi güzel verimlilik özellikleri sağlar.

## Eclipse

### Sbteclipse kurulumu

Eclipse ile entegrasyon [sbteclipse](https://github.com/typesafehub/sbteclipse) 4.0.0 ya da daha yeni sürüm gerektirir..

```scala
addSbtPlugin("com.typesafe.sbteclipse" % "sbteclipse-plugin" % "4.0.0")
```

`eclipse` komutunu çalıştırmadan önce `compile` komutunu çalıştırmalısını. `eclipse` komutunu aşağıdaki ayarları build.sbt dosyasına ekleyerek çalıştırdığınızda zorla derleme gerçekleştirebilirsiniz.

```scala
// Compile the project before generating Eclipse files, so that generated .scala or .class files for views and routes are present
EclipseKeys.preTasks := Seq(compile in Compile)
```

Eğer projenizde Scala kaynakları varsa, you will need to install [Scala IDE](http://scala-ide.org/) yüklemelisiniz.

Eğer SCALA IDE yüklemek istemiyorsanız ve projenizde yalnızca Java kaynakları varsa, aşağıdaki gibi ayarlayabilirsiniz.

```scala
EclipseKeys.projectFlavor := EclipseProjectFlavor.Java           // Java project. Don't expect Scala IDE
EclipseKeys.createSrc := EclipseCreateSrc.ValueSet(EclipseCreateSrc.ManagedClasses, EclipseCreateSrc.ManagedResources)  // Use .class files instead of generated .scala files for views and routes
```

### Konfigürasyon oluşturma

Play [Eclipse](https://eclipse.org/) konfigürasyonunu basitleştirmek için bir komut sağlar. Play uygulamasını çalışan bir Eclipse projesine dönüştürmek için, `eclipse` komutunu kullanınız. 

```bash
[my-first-app] $ eclipse
```

Eğer uygun kaynak jarlarını yakalamak istiyorsanız (Bu daha uzun sürecek ve bazı kaynakların kaybedilme ihtimali olacak):

```bash
[my-first-app] $ eclipse with-source=true
```

> Note if you are using sub-projects with aggregate, you would need to set `skipParents` appropriately in `build.sbt`:

```scala
EclipseKeys.skipParents in ThisBuild := false
```

or from the play console, type:

```bash
[my-first-app] $ eclipse skip-parents=false
```

You then need to import the application into your Workspace with the **File/Import/General/Existing project…** menu (compile your project first).

[[images/eclipse.png]]

To debug, start your application with `activator -jvm-debug 9999 run` and in Eclipse right-click on the project and select **Debug As**, **Debug Configurations**. In the **Debug Configurations** dialog, right-click on **Remote Java Application** and select **New**. Change **Port** to 9999 and click **Apply**. From now on you can click on **Debug** to connect to the running application. Stopping the debugging session will not stop the server.

> **Tip**: You can run your application using `~run` to enable direct compilation on file change. This way scala template files are auto discovered when you create a new template in `view` and auto compiled when the file changes. If you use normal `run` then you have to hit `Refresh` on your browser each time.

If you make any important changes to your application, such as changing the classpath, use `eclipse` again to regenerate the configuration files.

> **Tip**: Do not commit Eclipse configuration files when you work in a team!

The generated configuration files contain absolute references to your framework installation. These are specific to your own installation. When you work in a team, each developer must keep his Eclipse configuration files private.

## IntelliJ

[Intellij IDEA](https://www.jetbrains.com/idea/) lets you quickly create a Play application without using a command prompt. You don't need to configure anything outside of the IDE, the SBT build tool takes care of downloading appropriate libraries, resolving dependencies and building the project.

Before you start creating a Play application in IntelliJ IDEA, make sure that the latest [Scala Plugin](https://www.jetbrains.com/idea/help/creating-and-running-your-scala-application.html) is installed and enabled in IntelliJ IDEA. Even if you don't develop in Scala, it will help with the template engine and also resolving dependencies.

To create a Play application:

1. Open ***New Project*** wizard, select ***Activator*** under ***Scala*** section and click ***Next***.
2. Select one of the templates suitable. For the basic empty application you can select [Play Scala Seed](https://www.lightbend.com/activator/template/play-scala). The full list of templates can be found on [Lightbend Activator templates page](https://www.lightbend.com/activator/templates).
3. Enter your project's information and click ***Finish***.

You can also import an existing Play project.

To import a Play project:

1. Open Project wizard, select ***Import Project***.
2. In the window that opens, select a project you want to import and click ***OK***.
3. On the next page of the wizard, select ***Import project from external model*** option, choose ***SBT project*** and click ***Next***.
4. On the next page of the wizard, select additional import options and click ***Finish***.

Check the project's structure, make sure all necessary dependencies are downloaded. You can use code assistance, navigation and on-the-fly code analysis features.

You can run the created application and view the result in the default browser `http://localhost:9000`. To run a Play application:

1. Create a new Run Configuration -- From the main menu, select Run -> Edit Configurations
2. Click on the + to add a new configuration
3. From the list of configurations, choose "SBT Task"
4. In the "tasks" input box, simply put "run"
5. Apply changes and select OK.
6. Now you can choose "Run" from the main Run menu and run your application

You can easily start a debugger session for a Play application using default Run/Debug Configuration settings.

For more detailed information, see the Play Framework 2.x tutorial at the following URL:

<https://www.jetbrains.com/idea/help/getting-started-with-play-2-x.html>

### Navigate from an error page to the source code

Using the `play.editor` configuration option, you can set up Play to add hyperlinks to an error page.  This will link to runtime exceptions thrown when Play is running development mode.

> NOTE: Play can only display runtime exceptions, and compilation errors (even involving Twirl templates or routes) cannot be displayed in an error page. 

You can easily navigate from error pages to IntelliJ directly into the source code, by installing the [Remote Call IntelliJ plugin](https://github.com/Zolotov/RemoteCall).

Install the Remote Call plugin and run your app with the following options:

```
-Dplay.editor=http://localhost:63342/api/file/?file=%s&line=%s -Dapplication.mode=dev
```

## Netbeans

### Generate Configuration

Play does not have native [Netbeans](https://netbeans.org/) project generation support at this time, but there is a Scala plugin for NetBeans which can help with both Scala language and SBT:

<https://github.com/dcaoyuan/nbscala>

There is also a SBT plugin to create Netbeans project definition:

<https://github.com/dcaoyuan/nbsbt>

## ENSIME

### Install ENSIME

Follow the installation instructions at <https://github.com/ensime/ensime-emacs>.

### Generate configuration

Edit your project/plugins.sbt file, and add the following line (you should first check <https://github.com/ensime/ensime-sbt> for the latest version of the plugin):

```scala
addSbtPlugin("org.ensime" % "ensime-sbt" % "0.2.3")
```

Start Play:

```bash
$ activator
```

Enter 'gen-ensime' at the play console. The plugin should generate a .ensime file in the root of your Play project.

```bash
[MYPROJECT] $ gen-ensime
[info] Gathering project information...
[info] Processing project: ProjectRef(file:/Users/aemon/projects/www/MYPROJECT/,MYPROJECT)...
[info]  Reading setting: name...
[info]  Reading setting: organization...
[info]  Reading setting: version...
[info]  Reading setting: scala-version...
[info]  Reading setting: module-name...
[info]  Evaluating task: project-dependencies...
[info]  Evaluating task: unmanaged-classpath...
[info]  Evaluating task: managed-classpath...
[info] Updating {file:/Users/aemon/projects/www/MYPROJECT/}MYPROJECT...
[info] Done updating.
[info]  Evaluating task: internal-dependency-classpath...
[info]  Evaluating task: unmanaged-classpath...
[info]  Evaluating task: managed-classpath...
[info]  Evaluating task: internal-dependency-classpath...
[info] Compiling 5 Scala sources and 1 Java source to /Users/aemon/projects/www/MYPROJECT/target/scala-2.9.1/classes...
[info]  Evaluating task: exported-products...
[info]  Evaluating task: unmanaged-classpath...
[info]  Evaluating task: managed-classpath...
[info]  Evaluating task: internal-dependency-classpath...
[info]  Evaluating task: exported-products...
[info]  Reading setting: source-directories...
[info]  Reading setting: source-directories...
[info]  Reading setting: class-directory...
[info]  Reading setting: class-directory...
[info]  Reading setting: ensime-config...
[info] Wrote configuration to .ensime
```

### Start ENSIME

From Emacs, execute M-x ensime and follow the on-screen instructions.

That's all there is to it. You should now get type-checking, completion, etc. for your Play project. Note, if you add new library dependencies to your play project, you'll need to re-run "gen-ensime" and re-launch ENSIME.

### More Information

Check out the ENSIME README at <https://github.com/ensime/ensime-emacs>. If you have questions, post them in the ensime group at <https://groups.google.com/forum/?fromgroups=#!forum/ensime>.

## All Scala Plugins if needed

1. Eclipse Scala IDE: <http://scala-ide.org/>
2. NetBeans Scala Plugin: <https://github.com/dcaoyuan/nbscala>
3. IntelliJ IDEA Scala Plugin: <http://blog.jetbrains.com/scala/>
4. ENSIME - Scala IDE Mode for Emacs: <https://github.com/aemoncannon/ensime>
(see below for ENSIME/Play instructions)
