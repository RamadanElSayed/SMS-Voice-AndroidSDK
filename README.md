
    private class OTS_AsyncTask extends AsyncTask<String, Void, String> {

        @Override
        protected String doInBackground(String... params) {

            try {

                OTSRestClient client = new OTSRestClient("appSid");

                IMessageResource messageResource = client.getMessageResource();

                //Send Message 1
                MessageResponse sendResponse = messageResource.send(new MessageRequest("962789xxxxxx", "Content Message"));
                if (sendResponse.isSuccess()) {
                    Log.d("SendResponse:", "" + sendResponse);
                    //Do something
                } else {
                    Log.d("SendResponse Fail:", sendResponse.getErrorCode() + "-" + sendResponse.getMessage());
                }


                //Send Bulk 1
                //for multiple destination separated by commas.
                BulkRequest bulkRequest1 = new BulkRequest("962789xxxxxx", "Bulk Request Message 1");
                BulkResponse sendBulk1 = messageResource.sendBulk(bulkRequest1);
                Log.d("SendBulk1:", "" + sendBulk1);


                //Send Bulk 2 recipient in List Using BulkRequest Class
                List<String> recipients = new ArrayList<>();
                recipients.add("962789xxxxxx");
                recipients.add("962789xxxxxx");

                BulkRequest bulkRequest2 = new BulkRequest(recipients, "Bulk Request Message 2");
                BulkResponse sendBulk2 = messageResource.sendBulk(bulkRequest2);
                Log.d("SendBulk2:", "" + sendBulk2);


                //Send Bulk 3 recipient in Array Using BulkRequest Class
                String[] recipientArray = new String[2];
                recipientArray[0] = "962789xxxxxx";
                recipientArray[1] = "962789xxxxxx";
                BulkRequest bulkRequest3 = new BulkRequest(recipientArray, "Bulk Request Message 3");
                BulkResponse sendBulk3 = messageResource.sendBulk(bulkRequest3);
                Log.d("SendBulk3:", "" + sendBulk3);

                //Send Bulk 4 Using Map
                Map<String, String> bulkMap = new HashMap<>();
                bulkMap.put("Recipient", "962789xxxxxx");
                bulkMap.put("Body", "Bulk Request Message 4");
                BulkResponse sendBulk4 = messageResource.sendBulk(bulkMap);
                Log.d("SendBulk4:", "" + sendBulk4);


                //Send voice message
                IVoiceResource voiceResource = client.getVoiceResource();
                TTSCallRequest callRequest = new TTSCallRequest("96278xxxxxx", "Voice Message", CallLangConstant.LANG_ARABIC);
                TTSCallResponse callResponse = voiceResource.ttsCall(callRequest);


                //Get Message id
                String messageID = sendResponse.getMessageID();

                //Get MessageStatus
                MessageIDStatus messageIDStatus = messageResource.getMessageIDStatus(messageID);
                Log.d("MessageStatus:", "" + messageIDStatus);


                //GetMessagesDetails No Filter
                MessagesDetailsResponse messagesDetails = messageResource.getMessagesDetails();
                System.out.println("MessagesDetails:" + messagesDetails);

                //GetMessagesDetails 1 get 10 sent message detail
                MessagesDetailsRequest messagesDetailsRequest = new MessagesDetailsRequest();
                messagesDetailsRequest.setLimit(10);
                messagesDetailsRequest.setStatus(MessagesDetailsRequest.STATUS_SENT);
                MessagesDetailsResponse messagesDetails1 = messageResource.getMessagesDetails(messagesDetailsRequest);
                System.out.println("MessageDetails1:" + messagesDetails1);

                //or this way
                Map<String, String> map = new HashMap<String, String>();
                map.put("Limit", "10");
                map.put("Status", "Sent");
                MessagesDetailsResponse messagesDetails2 = messageResource.getMessagesDetails(map);
                System.out.println("MessageDetails2:" + messagesDetails2);


                //Message Report No Filter
                MessagesReportResponse messagesReport = messageResource.getMessagesReport();
                System.out.println("MessageReport:" + messagesReport);

                MessagesReportRequest mrr = new MessagesReportRequest();
                mrr.setStatus(MessagesReportRequest.STATUS_SENT);
                MessagesReportResponse messagesReport1 = messageResource.getMessagesReport(mrr);

                System.out.println("MessagesReport1:" + messagesReport1);

            } catch (IOException ex) {
                Logger.getLogger(MainActivity.class.getName()).log(Level.SEVERE, null, ex);
            }

            return null;
        }

        @Override
        protected void onPostExecute(String result) {     }

        @Override
        protected void onPreExecute() {   }
    }
