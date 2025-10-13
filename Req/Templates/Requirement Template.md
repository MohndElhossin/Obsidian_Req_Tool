<%*
/* 
  Smart Requirements Template with Filename-based IDs
*/
try {
  const currentFile = tp.file.find_tfile(tp.file.title);
  let existingContent = await app.vault.read(currentFile);
  
  // Get filename without extension for the prefix
  const fileName = tp.file.title.replace(/\.[^/.]+$/, ""); // Remove file extension
  const filePrefix = fileName.replace(/[^a-zA-Z0-9]/g, "").toUpperCase().substring(0, 15); // Clean and shorten
  
  // Find all REQ patterns in the current file
  const reqPattern = new RegExp(`## ${filePrefix}-REQ-(\\d+)`, 'g');
  let matches = [...existingContent.matchAll(reqPattern)];
  let maxCount = 0;
  
  if (matches.length > 0) {
    matches.forEach(match => {
      const num = parseInt(match[1]);
      if (num > maxCount) maxCount = num;
    });
  }
  
  // Next requirement number
  const count = maxCount + 1;
  const id = `${filePrefix}-REQ-` + count.toString().padStart(3, '0');
  
  // Generate the new requirement block
  let newRequirement = "";
  newRequirement += `## ${id}\n`;
  newRequirement += `Requirement_ID:: [[#${id}|${id}]]  \n`;
  newRequirement += `Feature Assignment::  \n`;
  newRequirement += `Requirement Type ::  \n`;
  newRequirement += `Priority:: Medium  \n`;
  newRequirement += `ObjectType:: Requirement  \n`;
  newRequirement += `**Description:**  \n\n`;
  newRequirement += `**Comments:**  \n`;
  newRequirement += `Status:: Draft  \n`;
  newRequirement += `Upstream::  \n`;
  newRequirement += `Downstream::  \n\n`;
  newRequirement += `---\n\n`;

  await app.vault.modify(currentFile, existingContent + newRequirement);
  new Notice(`Added ${id} to current file`);
  
  tR += ``;
  
} catch (error) {
  new Notice("Error: " + error);
  tR += `Error: ${error}`;
}
%>