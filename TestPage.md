<h2>Call for Grand Challenge Solutions</h2>
<p style="text-align: left;">The ACM DEBS 2016 Grand Challenge is the sixth in a series of challenges which seek to provide a common ground and uniform evaluation criteria for a competition aimed at both research and industrial event-based systems. The goal of the 2016 DEBS Grand Challenge competition is to evaluate event-based systems for real-time analytics over high volume data streams in the context of graph models.</p>
<p style="text-align: left;">The underlying scenario addresses the analysis metrics for an evolving social-network graph. Specifically, the 2016 Grand Challenge targets the following problems: (1) identification of the posts that currently trigger the most activity, and (2) identification of large communities that are currently involved in a topic. The corresponding queries require continuous analysis of an evolving graph structure under the consideration of multiple streams that reflect updates to the graph.</p>
<p style="text-align: left;">The data for the DEBS 2016 Grand Challenge is based on the dataset provided together with the LDBC (<a herf="http://ldbcouncil.org/">http://ldbcouncil.org/</a>) Social Network Benchmark. It takes up the general scenario from the 2014 SIGMOD Programming contest but - in contrasts to the SIGMOD contest - explicitly focuses on processing streaming data. Details about the data, the queries for the Grand Challenge, and information about how the challenge will be evaluated are provided below.</p>
<h3>Prize</h3>
<p style="text-align: left;">Participants in the 2016 DEBS Grand Challenge will have the chance to win two prizes. The first prize is the “Grand Challenge Award” for the best performing submission. The winner (or winning team) of the “Grand Challenge Award” will be additionally awarded with a monetary reward of $1000. The monetary reward and the whole DEBS 2016 Grand Challenge is sponsored by the WSO2 (<a href="http://wso2.com/">http://wso2.com/</a>).</p>
<p>The second prize is the “Grand Challenge Audience Award” - it is determined based on the audience feedback provided during the DEBS 2016 conference. The “Grand Challenge Audience Award”, as opposed to the overall “Grand Challenge Award”, does not entail any additional monetary reward.</p>
<h3>License</h3>
<p style="text-align: left;">All solutions submitted to the DEBS 2016 Grand Challenge must be made open source under the BSD license: <a href="https://opensource.org/licenses/BSD-3-Clause">https://opensource.org/licenses/BSD-3-Clause</a>. A solution incorporates concepts, queries, and code developed for the purpose of solving the Grand Challenge. If a solution is developed within the context of, is built on top of, or is using an existing system or solution which is licensed under a different license than BSD, then such an existing solution or system needs not have its license changed.</p>

<h3>Input Data Streams</h3>
<p>The input data is organized in four separate streams, each provided as a text file. Namely, we provide the following input data files:</p>

<p><b>friendships.dat</b>: &lt;ts, user_id_1, user_id_2&gt;</p>
    <div class="std_table">
    <table>
    <tbody>
    <tr>
    <td style="width: 30%;">ts</td>
    <td>is the friendship's establishment timestamp</td>
    </tr>
    <tr>
    <td style="width: 30%;">user_id_1</td>
    <td>is the id of one of the users</td>
    </tr>
    <tr>
    <td style="width: 30%;">user_id_2</td>
    <td>is the id of the other user</td>
    </tr>
    </tbody>
    </table>
    </div>

<p><b>posts.dat</b>: &lt;ts, post_id, user_id, post, user&gt;</p>
    <div class="std_table">
    <table>
    <tbody>
    <tr>
    <td style="width: 30%;">ts</td>
    <td>is the post's timestamp</td>
    </tr>
    <tr>
    <td style="width: 30%;">post_id</td>
    <td>is the unique id of the post</td>
    </tr>
    <tr>
    <td style="width: 30%;">user_id</td>
    <td>is the unique id of the user</td>
    </tr>
    <tr>
    <td style="width: 30%;">post</td>
    <td>is a string containing the actual post content</td>
    </tr>
    <tr>
    <td style="width: 30%;">user</td>
    <td>is a string containing the actual user name</td>
    </tr>
    </tbody>
    </table>
    </div>

<p><b>comments.dat</b>: &lt;ts, comment_id, user_id, comment, user, comment_replied, post_commented2&gt;</p>
    <div class="std_table">
    <table>
    <tbody>
    <tr>
    <td style="width: 30%;">ts</td>
    <td>is the comment's timestamp</td>
    </tr>
    <tr>
    <td style="width: 30%;">comment_id</td>
    <td>is the unique id of the comment</td>
    </tr>
    <tr>
    <td style="width: 30%;">user_id</td>
    <td>is the unique id of the user</td>
    </tr>
    <tr>
    <td style="width: 30%;">comment</td>
    <td>is a string containing the actual comment</td>
    </tr>
    <tr>
    <td style="width: 30%;">user</td>
    <td>is a string containing the actual user name</td>
    </tr>
    <tr>
    <td style="width: 30%;">comment_replied</td>
    <td>is the id of the comment being replied to (-1 if the tuple is a reply to a post)</td>
    </tr>
    <tr>
    <td style="width: 30%;">post_commented</td>
    <td>is the id of the post being commented (-1 if the tuple is a reply to a comment)</td>
    </tr>
    </tbody>
    </table>
    </div>

<p><b>likes.dat</b>: &lt;ts, user_id, comment_id&gt;</p>
    <div class="std_table">
    <table>
    <tbody>
    <tr>
    <td style="width: 30%;">ts</td>
    <td>is the like's timestamp</td>
    </tr>
    <tr>
    <td style="width: 30%;">user_id</td>
    <td>is the id of the user liking the comment</td>
    </tr>
    <tr>
    <td style="width: 30%;">comment_id</td>
    <td>is the id of the comment</td>
    </tr>
    </tbody>
    </table>
    </div>


<p>Please note that as the contents of files represent data streams, each file is sorted based on its respective timestamp field.</p>

<p>A sample set of input data streams can be downloaded from this URL: <a href="https://www.dropbox.com/s/vo83ohrgcgfqq27/data.tar.bz2">https://www.dropbox.com/s/vo83ohrgcgfqq27/data.tar.bz2</a>. These files are only meant for development and debugging. Our tests will be based on other files (possibly of larger size), but strictly following the same format.


<h3>Query 1</h3>
<p>The goal of query 1 is to compute the top-3 scoring active posts, producing an updated result every time they change.</p>

<p>The total score of an active post P is computed as the sum of its own score plus the score of all its related comments. Active posts having the same total score should be ranked based on their timestamps (in descending order), and if their timestamps are also the same, they should be ranked based on the timestamps of their last received related comments (in descending order). A comment C is related to a post P if it is a direct reply to P or if the chain of C's preceding messages links back to P.</p>

<p>Each new post has an initial own score of 10 which decreases by 1 each time another 24 hours elapse since the post's creation. Each new comment's score is also initially set to 10 and decreases by 1 in the same way (every 24 hours since the comment's creation). Both post and comment scores are non-negative numbers, that is, they cannot drop below zero. A post is considered no longer active (that is, no longer part of the present and future analysis) as soon as its total score reaches zero, even if it receives additional comments in the future.</p>

<<<<<<< HEAD
<h4>Input Streams: posts, comments</h4>
=======
<p><strong>Input Streams:</strong> <em>posts</em>, <em>comments</em></p>
>>>>>>> 9e3a36bacb9a50a79763fa9c57d3b6b37c98269a

<h4>Output specification:</h4>
<p style="text-align: left;"><code>&lt;ts,top1_post_id,top1_post_user,top1_post_score,top1_post_commenters,<br/>top2_post_id,top2_post_user,top2_post_score,top2_post_commenters,<br/>top3_post_id,top3_post_user,top3_post_score,top3_post_commenters&gt;</code><br><code></code></p>

<p><strong>ts:</strong> the timestamp of the tuple that triggers a change in the top-3 scoring active posts appearing in the rest of the tuple<br/>
<strong>topX_post_id:</strong> the unique id of the top-X post<br/>
<strong>topX_post_user:</strong> the user author of top-X post<br/>
<strong>topX_post_commenters:</strong> the number of commenters (excluding the post author) for the top-X post</p>

<p>Results should be sorted by their timestamp field. The character "-" (a minus sign without the quotations) should be used for each of the fields (post id, post user, post commenters) of any of the top-3 positions that has not been defined. Needless to say, the logical time of the query advances based on the timestamps of the input tuples, not the system clock.</p>

<h4>Sample output tuples for the query</h4>
<p style="text-align: left;"><code>2010-09-19 12:33:01,25769805561,Karl Fischer,115,10,25769805933,Chong Liu,83,4,-,-,-,-<br/>
2010-10-09 21:55:24,34359739095,Karl Fischer,58,7,34359740594,Paul Becker,40,2,34359740220,Chong Zhang,10,0<br/>
2010-12-27 22:11:54,42949673675,Anson Chen,127,12,42949673684,Yahya Abdallahi,69,8,42949674571,Alim Guliyev,10,0
</code></p>



<h3>Query 2</h3>
<p>This query addresses the change of interests with large communities. It represents a version of query type 2 from the 2014 SIGMOD Programming contest. Unlike in the SIGMOD problem, the version for the DEBS Grand Challenge focuses on the dynamic change of query results over time, i.e., calls for a continuous evaluation of the results.</p>

<p><strong>Goal of the query:</strong><br/>
Given an integer <em>k</em> and a duration <em>d</em> (in seconds), find the <em>k</em> comments with the largest range, where the range of a comment is defined as the size of the largest connected component in the graph defined by persons who (a) have liked that comment (see <em>likes</em>, <em>comments</em>), (b) where the comment was created not more than <em>d</em> seconds ago, and (c) know each other (see <em>friendships</em>).</p>

<p><strong>Parameters:</strong> <em>k</em>, <em>d</em></p>
<p><strong>Input Streams:</strong> <em>likes</em>, <em>friendships</em>, <em>comments</em></p>

<p><strong>Output: </strong><br/>The output includes a single timestamp <em>ts</em> and exactly <em>k</em> strings per line. The timestamp and the strings should be separated by commas. The <em>k</em> strings represent comments, ordered by range from largest to smallest, with ties broken by lexicographical ordering (ascending). The <em>k</em> strings and the corresponding timestamp must be printed only when some input triggers a change of the output, as defined above. If less than k strings can be determined, the character “-” (a minus sign without the quotations) should be printed in place of each missing string.</p>
<p>The field <em>ts</em> corresponds to the timestamp of the input data item that triggered an output update. For instance, a new friendship relation may change the size of a community with a shared interest and hence may change the <em>k</em> strings. The timestamp of the event denoting the added friendship relation is then the timestamp <em>ts</em> for that line's output. Also, the output must be updated when the results change due to the progress of time, e.g., when a comment is older that d. Specifically, if the update is triggered by an event leaving a time window at <em>t2</em> (i.e., <em>t2</em> = timestamp of the event + window size), the timestamp for the update is <em>t2</em>. As in Query 1, it is needless to say that timestamps refer to the logical time of the input data streams, rather than on the system clock.
</p>
<p>In summary, the output is specified as follows:</p>
<p><b>ts</b>: the timestamp of the tuple that triggers a change in the top-3 scoring active posts<br/>
<b>comments_1,...,comment_k</b>: top k comments ordered by range, starting with the largest range (comment_1). </p>

<h4>Sample output tuples for the query with k=3 could look as follows:</h4>

<p><code>2010-10-28T05:01:31.022+0000,I love strawberries,-,-<br/>
2010-10-28T05:01:31.024+0000,I love strawberries,what a day!,-<br/>
2010-10-28T05:01:31.027+0000,I love strawberries,what a day!,well done<br/>
2010-10-28T05:01:31.032+0000,what a day!,I love strawberries,well done</code></p>


<h3 style="text-align: left;">Submission Information</h3>
<p style="text-align: left;">For general submission guidelines (incl. paper formatting) please refer to <a href="submission-guidelines.html">http://www.debs2016.org/submission-guidelines.html</a>.</p>
<p style="text-align: left">Below we provide the specific submissions guidelines for the Grand Challenge.</p>
<p style="text-align: left;"><strong>Step 1</strong> - submission of non-binding intent for participation. The goal of this submission is to initiate the contact between the DEBS Challenge organizers and solution authors. The submission of the intent should be in form of an abstract and done via the EasyChair (<a href="https://easychair.org/conferences/?conf=debs2016">https://easychair.org/conferences/?conf=debs2016</a>) choosing the Grand Challenge Track. There is no deadline for this submission. <strong><span style="color: #ff0000;">Please note that not registering your non-binding intent might result in missing of the website update notifications.</span></strong></p>
<p style="text-align: left;"><strong>Step 2</strong> - 15<sup>th</sup> of January - Evaluation platform will be made available to registered participants. Registered participants will receive detailed instructions about how to provide the virtual machine with their solutions and receive the evaluation results.</p>
<p style="text-align: left;"><strong>Step 3</strong> - 3<sup>rd</sup> of April - deadline for submissions of the DEBS Challenge solutions. The evaluation platform will be closed at this time. Final results will be sent to each participant (each participant's results refer to their last submitted solution).</p>
<p style="text-align: left;"><strong>Step 4</strong> - 10<sup>th</sup> of April - notification of solution acceptance. All accepted solutions will be ranked based on their performance, the best solutions will be invited to submit either a long or short paper (based on their ranking) discussing their approach. In addition, the best solutions will get a presentation slot in the DEBS Grand Challenge Track at the conference or the opportunity to present a poster in the poster session (dependent on their ranking).</p>
<p style="text-align: left;"><strong>Step 5</strong> - 1<sup>st</sup> of May - provision of the Camera Ready for the accepted solutions.</p>
<p style="text-align: left;"><strong>Step 6</strong> - June 20 - June 24 - Announcement of the DEBS 2016 Grand Challenge winner during the conference. DEBS Grand Challenge winner will be the solution achieving the shortest answer time for both queries. In addition, an “Audience Award” will be granted to the solution which appears most interesting to the participants on the DEBS conference. It will be determined through a vote on site and factor in the presentations during the DEBS Grand Challenge track or poster session.</p>
<h3>Important Dates</h3>
<div class="std_table">
<table>
<tbody>
<tr><td>15<sup>th</sup> December</td><td>Challenge announcement</td></tr>
<tr><td>15<sup>th</sup> January</td><td>Evaluation platform made available to registered participants</td></tr>
<tr><td>3<sup>rd</sup> April</td><td>Evaluation period ends</td></tr>
<tr><td>10<sup>th</sup> April</td><td>Notification of acceptance</td></tr>
<tr><td>1<sup>st</sup> May</td><td>DEBS Camera Ready deadline</td></tr>
</tbody>
</table>
</div>
<h3>Evaluation</h3>
<p>The evaluation of the Grand Challenge will be done using VirtualBox virtual machines. An automated evaluation platform will be used for this year's challenge. The evaluation platform will be made available on the 15th of January. Registered participants will be able to run their code at the evaluation platform and receive the performance results multiple times (detailed instructions about the submission process will be shared  with participants starting from the 15th of January once they register to the challenge via easychair). The evaluation platform will be closed on the 3rd of April (challenge deadline).</p>
<p>For both queries it is assumed that the result is calculated in a streaming fashion - i.e.: (1) solutions must not make use of any pre-calculated information, such as indices and (2) result streams must be updated continuously.</p>
<p>The virtual machine should be setup with 4 cores and 8GB of RAM.</p>
<p>The ranking of the submissions will be done using the total duration of the execution and the average delay per stream. Specifically, we will rank the solutions (a) by the total execution time as well as (b) by the average latency (combining the average results from both streams in a sum). The average latency should be computed as the average of each output tuple latency, the latter measured as the difference between the system clock taken when logging an output tuple and the system clock taken when the processing of the input tuple triggering the result starts. The final ranking will be determined by a score that is the sum of the rank for execution time and the rank for delay. For solutions with the same overall score we will favor the solution with the lower execution time.</p>
<p> </p>
<h3>FAQ</h3>
<p>FAQ will appear here shortly.</p>
<p> </p>
<h3>Grand Challenge Co-Chairs</h3>
<p style="text-align: left;">Vincenzo Gulisano, Chalmers University of Technology, Sweden</p>
<p style="text-align: left;">Zbigniew Jerzak, SAP SE, Germany</p>
<p style="text-align: left;">Spyros Voulgaris, Vrije Universiteit Amsterdam, The Netherlands</p>
<p style="text-align: left;">Holger Ziekow, Furtwangen University, Germany</p>
