# 🚀 Hackathon Registration Website:

This is a Hackathon Registration Website designed and developed during my college period. The website provides a user-friendly registration platform for participants to register for the hackathon seamlessly.

# ✨ Tech Stack:

React.js – For building the interactive UI

Tailwind CSS – For responsive and modern styling

Framer Motion – For smooth animations and transitions

Google Web API – For handling and storing registration data

# 🎯 Features:
Simple & Interactive UI – Ensures an easy and smooth registration process

Responsive Design – Works well on mobile, tablet, and desktop

Dynamic Animations – Uses Framer Motion for a visually engaging experience

Google Web API Integration – Securely stores participant data

# 📌 Project Overview:

During my college journey, we organized a Hackathon, and I played a key role in designing and developing this website. The main goal was to create a seamless registration process with an intuitive interface.

# 🎥 Demo Video:
https://github.com/user-attachments/assets/947aea5f-a4b5-4681-ba36-d983fcfbf5f2


# Google Sheet
function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    const data = e.parameter;
    
    // Create folder in Google Drive if it doesn't exist
    const folderName = "Registration Form Submissions";
    let folder = DriveApp.getFoldersByName(folderName);
    let mainFolder;
    
    if (folder.hasNext()) {
      mainFolder = folder.next();
    } else {
      mainFolder = DriveApp.createFolder(folderName);
    }
    
    // Handle PDF upload
    let pdfUrl = "";
    if (data.Project_pdf) {
      const fileBlob = Utilities.newBlob(
        Utilities.base64Decode(data.Project_pdf.split(',')[1]),
        'application/pdf',
        ${data.Project_teamName}_${data.Project_projectTitle}.pdf
      );
      const file = mainFolder.createFile(fileBlob);
      file.setSharing(DriveApp.Access.ANYONE_WITH_LINK, DriveApp.Permission.VIEW);
      pdfUrl = file.getUrl();
    }
    
    const rowData = [
      new Date(),  // Timestamp
      // Member 1
      data.Member1_name,
      data.Member1_email,
      data.Member1_department,
      data.Member1_phone,
      data.Member1_year,
      // Member 2
      data.Member2_name,
      data.Member2_email,
      data.Member2_department,
      data.Member2_phone,
      data.Member2_year,
      // Member 3
      data.Member3_name,
      data.Member3_email,
      data.Member3_department,
      data.Member3_phone,
      data.Member3_year,
      // Project Details
      data.Project_teamName,
      data.Project_projectTitle,
      data.Project_topic,
      pdfUrl  // PDF file link
    ];
    
    sheet.appendRow(rowData);
    
    return ContentService.createTextOutput(JSON.stringify({ 
      "result": "success",
      "message": "Data saved successfully" 
    }))
    .setMimeType(ContentService.MimeType.JSON);
    
  } catch (error) {
    return ContentService.createTextOutput(JSON.stringify({ 
      "result": "error",
      "message": error.toString()
    }))
    .setMimeType(ContentService.MimeType.JSON);
  }
}
