<html>
<head>
    <title>Upload and Download</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">        
</head>
<body>
<div id="main">

<!-- CLOCK. Not finished : at 00h00 delete all the files and restart the count. -->
<!-- Probably better to do it on PureData, running h24 -->
        <?php 
            date_default_timezone_set('Europe/Paris');
            // echo date('Y-m-d H:i:s');

            // $clock = time(); //time() permet d

            echo "<br/>" . $clock;

            // $date == $date2 => false
            // $date === $date2 => false
        ?>

<!-- HTML GENERATION PART -->
<!-- HTML PHP DOIT ÊTRE AVANT LES APPELS DE FONTION TEXTE --> 
        <?php
            //First form : consent
            $Consent = <<<HTML
                <div id="Consent" style="display:none">
                    <form method="post" action="Code_ToutenUn.php" id="ConsentForm">
                                <!-- Cases à cocher -->
                        <input type="checkbox" name="accept" id="accept" checked="checked" />
                        <label for="accept"> I consent to render public my file,</br> in the area of 2m square.</label> </br>
                        <input type="checkbox" name="content" id="content" checked="checked"/>
                        <label for="content"> I don't share sexual/ explicit/ violent content.</label> </br>
                        <input type="checkbox" name="iA" id="iA" checked="checked"/>
                        <label for="iA"> The content that I share is not Ai generated.</label> </br>
                        <input type="checkbox" name="prohibe" id="prohibe" checked="checked"/>
                        <label for="prohibe"> I never share prohibited content.</label> </br></br>
                        <input id="SendConsent" type="submit" value="Send Consent"/>

                    </form>
                </div>
            HTML;

            //Second form : send files
            $SendFichier = <<<HTML
                <div id="formulaireFichier">
                
                    <form id="SendFile" action="Code_ToutenUn.php" method="post" enctype="multipart/form-data">
                        <p>  </br></br>
                        <input id="Infos" type="text" name="Title" placeholder="Title" maxlength="25"/> <br/><br/>
                        <textarea name="Description" id="Infos" rows="10" cols="20" placeholder="Short Description, 200 character max" maxlength="200">Short Description, 200 character max</textarea><br/><br/>
                        <input type="file" name="monfichier" id="ChoixFichier"/></br>
                        <input type="submit" value="Send File" id="Infos"/>
                        </p>
                        
                    </form>
                </div>
            HTML;

            //Intro
            $Intro = <<<HTML
                <div id="MainTitle">
                    <img src="Img/GVJoseph.svg" width=250px>
                </div>
                <div id="Intro"><div id="Texte">
                    <p> Inside the imperialist sound of the tower,  we have built this space to share and exchange content, permitting to us to exist.
                    Capitalism want to erase our past, our memory, our culture. 
                    Together, we can create an alternative space, by and for us, outside of the big data, the state and the capitalism.</br>
                    Each user are limited to 1 file per day, limited to 2Mo, and must respect the rules of the community.
                    <input id="SentNext" type="submit" value="SentNext" onclick="show('Consent')"/>
                    </div></div>
            HTML;

            $PersonnalOrPublicFile = <<<HTML
                <div id="Choice">
                    <input id="Personal" type="submit" value="Personal" onclick="show('DownloadPersonalFile')"/>
                    <input id="Public" type="submit" value="Public" onclick="show('DownloadUserFile')"/>
                </div>
            HTML;
        ?>

<!-- COUNTER PHP -->
        <?php
            session_start();
            $fichier = 'compteur.txt'; 
            $tableau=unserialize ( urldecode ( file_get_contents ( "tableau.txt" ) ) );;

            $Origine = <<<HTML
                <a href='uploads/$file' download='$file'> $file</a><br>;
            HTML;        

            
                    $compteur = (int)file_get_contents($fichier);
                    $compteurFile = $compteur%100;
            
                    //If first time on the page, permit to send the form, if not, only the intro annd the download part (not implement yet)
                    echo $Intro;
                    if (isset($_POST['accept'])== 'on' && isset($_POST['content'])== 'on' && isset($_POST['iA'])== 'on' && isset($_POST['prohibe'])== 'on') {
                        echo $SendFichier;
                        //Debug : delete all the files on the folder :
                        //For delete all the repository
                        // rmdirRecursive('uploads');
                    }
                    else {
                        echo $Consent;
                    }
    
            //Deleting all the file and write a new empty array if there is 100 file in the folder
            // Now I've implement compteurFile, I need to use it rather than compteur, permitting to have the total amount of participant with compteur and the number of file in the folder with compteurFile
            if ($compteur % 100 ==0){
                rmdirRecursive('uploads');
                $tableau=array();
                file_put_contents ( "tableau.txt" , urlencode ( serialize ( $tableau ) ) );
                $compteur=0;
                file_put_contents($fichier, $compteur);

            }
            echo "<div> Amount of contribution :  $compteur.</br> Thanks for your contribution. </br> </br></div>";
            echo $PersonnalOrPublicFile;
            if (isset($_FILES['monfichier']) AND $_FILES['monfichier']['error'] == 0)
            {
                //'monfichier' est le 'name' de l'input de l'envoie du fichier du document Code.php
                //Test de taille de fichier
                    if ($_FILES['monfichier']['size'] <= 1000000){
                    //La taille est en octets, ici équivalent à 1Mo
                    //Test de l'extension
                        //Récupération de l'extension du fichier
                        $infosfichier = pathinfo($_FILES['monfichier']['name']);//Récupère les infos
                        $extension_upload = $infosfichier['extension']; //On les stockes dans un tableau
                    
                //File extension, not autorize images, only documents. 
                        $extensions_autorisees = array('png', 'pdf','pd', 'txt', 'doc'); //Les extensions autorisées
                        $name = $infosfichier['filename'];
                        $file = '' .$compteurFile. '.' .$extension_upload;
                        //On test les extensions autorisées
                        if (in_array($extension_upload, $extensions_autorisees)){ 
                            //On valide et stocke définitivement le fichier
                            //move_uploaded_file($_FILES['monfichier']['tmp_name'], 'uploads/' . basename($_FILES['monfichier']['name'])); //Ici le .basename est le nom que prends le fichier lors de l'upload. Ici son nom d'origine.
                            move_uploaded_file($_FILES['monfichier']['tmp_name'], 'uploads/' . $file);;
                        
                            // echo $compteur;
                            $tableau[$compteurFile ] = [$_POST['Title'], $_POST['Description'], '<a href="uploads/'.$file.'" download="'.$file.'"> '.$file.'</a>'];
                            file_put_contents ( "tableau.txt" , urlencode ( serialize ( $tableau ) ) );
                            if (!isset($_SESSION['visite_comptee'])) {
                                        $compteur = (int)file_get_contents($fichier);
                                        $compteur++;
                                        file_put_contents($fichier, $compteur);
                                        $_SESSION['visite_comptee'] = true; // Marque la session comme comptée
                                        $compteur = (int)file_get_contents($fichier);
                                        echo $Intro;
                            }
                        
                        }
                        else{
                            echo "Not authorized extension.";
                        }
                    }
                    else {
                        echo "File to heavy.";
                    }
            }
        ?>


        <div id="DownloadPersonalFile" style="display:none">
                <p></br></br></br></p>
                <table>
                <colgroup><col width="100"><col width="200"><col width="50"></colgroup>
                <thead><th> Title </th>
                <th> Infos </th>
                <th> Download </th></thead>
                <tr><td> La Tourelle Doc </td> <td> Documentation file, with the explaination</td><td></td></tr>
                <tr><td></td><td>///////////////////////////////////////////</td></tr>

                <tr><td> Pure Data Patch </td><td> The Sound of the installation is generated by PureData in function of the files present in the folder 'upload', uploaded by the public on this website.</td><td> <a href="LaTourelle.pd"> PD_Patch </a></td></tr>
                <tr><td></td><td>///////////////////////////////////////////</td></tr>
                
                <td> Short range Web </td><td>C# Code for run a webserver and a webpage on ESP32, shortrange and offGrid</td><td> </td></tr>

                <tr><td></td><td>///////////////////////////////////////////</td></tr>
                <td> Reticulum Resources </td><td> Long range Off Grid Communication</td><td> <a href="https://awesome-reticulum.net/">Awesome</a> </td></tr>
                <tr><td></td><td>///////////////////////////////////////////</td></tr>
                </tr>
                <!-- <td><div id="tableau">Hello</div></td> -->
                </table>
        </div> 

        <!-- Corriger : lorsque je clique la première fois ça ne cache pas la seconde partie -->
        <div id="DownloadUserFile" style="display:inline-block">
            <?php
                $tableau=array();
                $tableau = unserialize ( urldecode ( file_get_contents ( "tableau.txt" ) ) );
                $col ='Title, Infos, Download';
                echo "<p></br></br></br></p>";
                echo utilHtmlTable($col, $tableau);
                // print_r($tableau);
                    // foreach($tableau as $valeur) {
                    //     echo $valeur ,'<br/>';
                    // }
                    // print_r($tableau);
            ?>
        </div>
</div>
    
<?php

  function utilHtmlTable($cols, $vals, $params = ''){ //By jonathan84                                                                                            
        $cols = explode(',', $cols);
        $data = '<table'.$params.'><colgroup><col width="100"><col width="200"><col width="50"></colgroup> <thead><tr>';
        foreach($cols as $v){
            $data.= '<th>'.$v.'</th>';
        }
        $data.= '</tr></thead><tbody>';
        foreach($vals as $v){
            $data.= '<tr height="50px">';
            foreach($v as $v2){
                $data.= '<td><div id="tableau">'.$v2.'</div></td>';
            }
            $data.= '</tr><td></td><td>///////////////////////////////////////////</td></tr>';
        }
        $data.= '</tbody>';
        
        $data.= '</table>';
        return $data;
    }

    //For delete the entire data folder 
    function rmdirRecursive($dir) : void
    {
        foreach (glob($dir . "/*") as $item) {
            if (is_dir($item)) {
                rmdirRecursive($item);           
                rmdir($item); 
            } else {
                unlink($item);
            }
        }
    }
?>


<script>
let currentOpen = null;

function show(id) {
  if (currentOpen) {
    document.getElementById(currentOpen).style.display = "none";
  }
  
  if (currentOpen === id) {
    currentOpen = null;
  } else {
    document.getElementById(id).style.display = "inline-block";
    currentOpen = id;
  }
}
</script>
